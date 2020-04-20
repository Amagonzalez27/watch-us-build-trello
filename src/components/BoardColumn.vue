<template>
  <AppDrop @drop="moveTaskOrColumn">
    <AppDrag
      class="column"
      :transferData="{
        type: 'column',
        fromColumnIndex: columnIndex,
      }"
    >
      <div class="flex items-center mb-2 font-bold">{{ column.name }}</div>
      <div class="list-reset">
        <ColumnTask
          v-for="(task, $taskIndex) of column.tasks"
          :key="$taskIndex"
          :task="task"
          :taskIndex="$taskIndex"
          :columnIndex="columnIndex"
          :column="column"
          :board="board"
        />
        <input
          type="text"
          class="block pt-2 w-full bg-transparent"
          placeholder="+Enter new task"
          @keyup.enter="createTask($event, column.tasks)"
        />
      </div>
    </AppDrag>
  </AppDrop>
</template>

<script>
import ColumnTask from './ColumnTask';
import AppDrag from './AppDrag';
import AppDrop from './AppDrop';
import movingTasksAndColumnsMixin from '@/mixins/movingTasksAndColumnsMixin.js';

export default {
  mixins: [movingTasksAndColumnsMixin],
  components: {
    ColumnTask,
    AppDrag,
    AppDrop,
  },
  methods: {
    createTask(e, tasks) {
      this.$store.commit('CREATE_TASK', {
        tasks,
        name: e.target.value,
      });
      e.target.value = '';
    },
  },
};
</script>

<style lang="css">
.column {
  @apply bg-grey-light p-2 mr-4 text-left shadow rounded;
  min-width: 350px;
}
</style>
