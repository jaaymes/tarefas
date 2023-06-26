<template>
  <div id="app">
    <TaskProgress :progress="progress" />
    <NewTaskVue @taskAdded="addTask" />
    <TaskGrid
      :tasks="tasks"
      @taskDeleted="taskDeleted"
      @taskStateChange="taskStateChange"
    />
  </div>
</template>

<script>
import NewTaskVue from "./components/NewTask.vue";
import TaskGrid from "./components/TaskGrid.vue";
import TaskProgress from "./components/TaskProgress.vue";
import api from "./services/api";
import { v4 as uuidv4 } from "uuid";
export default {
  components: {
    TaskGrid,
    NewTaskVue,
    TaskProgress,
  },
  data() {
    return {
      tasks: [],
    };
  },
  computed: {
    progress() {
      const total = this.tasks.length;
      const done = this.tasks.filter((t) => !t.pending).length;
      return Math.round((done / total) * 100) || 0;
    },
  },
  methods: {
    async loadData() {
      const response = await api.get("/tasks");
      this.tasks = response.data;
    },
    async addTask(task) {
      const sameName = (t) => t.name === task.name;
      const reallyNew = !this.tasks.some(sameName);
      console.log("🚀 ~ file: App.vue:40 ~ addTask ~ reallyNew:", reallyNew);
      if (reallyNew) {
        const taskData = {
          id: uuidv4(),
          name: task.name,
          pending: true,
        };
        await api.post("/tasks", taskData);
        this.loadData();

        this.tasks.push(task);
        return;
      }
      this.$toast.error("Tarefa já existe", {
        position: "top-right",
      });
    },
    async taskDeleted(id) {
      await api.delete(`/tasks/${id}`);
      this.tasks.splice(id, 1);
    },
    async taskStateChange(id) {
      const task = this.tasks.find((t) => t.id === id);
      task.pending = !task.pending;
      await api.put(`/tasks/${id}`, task);
    },
  },
  mounted() {
    this.loadData();
  },
};
</script>



<style >
body {
  font-family: "Lato", sans-serif;
  background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
  color: #fff;
}

#app {
  display: flex;
  flex: 1;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

#app h1 {
  margin-bottom: 5px;
  font-weight: 300;
  font-size: 3rem;
}
</style>
