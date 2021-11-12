<template>
  <div class="todo-list mt-3">
    <Task
        v-for="task in taskList"
        :key="task.id" :task="task"
        @deleteTask="(taskId) => $emit('deleteTask', taskId)"
        @editTask="(taskId) => $emit('editTask', taskId)"
    />
  </div>
</template>

<script>
import Task from "./Task";

export default {
  name: "TaskList",

  props: {
    taskList: {
      type: Array,
      required: true,
      default: () => []
    }
  },

  components: {
    Task,
  },
};
</script>

<style scoped></style>
