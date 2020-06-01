<template>
  <AppDrop
    @drop="moveTaskOrColumn"
  >
    <AppDrag
      class="column"
      :transferData="{
        type: 'column',
        fromColumnIndex: columnIndex
      }"
    >
      <div class="flex items-center mb-2 font-bold">
        {{ column.name }}
      </div>
      <div class="list-reset">
        <BoardColumnTask
          v-for="(task, $taskIndex) of column.tasks"
          :key="$taskIndex"
          :task="task"
          :taskIndex="$taskIndex"
          :column="column"
          :columnIndex="columnIndex"
          :board="board"
        />

        <input
          type="text"
          class="block p-2 w-full bg-transparent"
          placeholder="+ Enter new task"
          @keyup.enter="createTask($event, column.tasks)"
        />
      </div>
    </AppDrag>
  </AppDrop>
</template>

<script>
import BoardColumnTask from './BoardColumnTask'
import moveTaskAndColumnMixin from '../mixins/moveTaskAndColumnMixin'
import AppDrag from './AppDrag'
import AppDrop from './AppDrop'

export default {
  mixins: [moveTaskAndColumnMixin],
  components: {
    BoardColumnTask,
    AppDrag,
    AppDrop
  },
  name: 'BoardColumn',
  methods: {
    createTask (e, tasks) {
      this.$store.commit('CREATE_TASK', {
        tasks,
        name: e.target.value
      })
      e.target.value = ''
    }
  }
}
</script>

<style lang="css">
  .column {
    @apply bg-grey-light p-2 mr-4 text-left shadow rounded;
    min-width: 350px;
  }
</style>
