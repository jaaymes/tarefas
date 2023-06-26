<template>
  <div class="task-grid">
    <template v-if="tasks.length">
      <Task
        class="task"
        v-for="task in tasks"
        :key="task.id"
        :task="task"
        @taskDeleted="$emit('taskDeleted', task.id)"
        @taskStateChange="$emit('taskStateChange', task.id)"
      />
    </template>
    <p v-else class="no-task">Quais as Tarefas de Hoje?</p>
  </div>
</template>

<script>
import Task from "./Task.vue";
export default {
  components: {
    Task,
  },
  props: {
    tasks: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped>
.task-grid {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}
.task-grid .task {
  margin: 10px;
}
.no-task {
  color: #aaa;
  font-size: 1.7rem;
}
</style>