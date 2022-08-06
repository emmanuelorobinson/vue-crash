<template>
  <AddTask v-if="showAddTask" @add-task="addTask" />
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from "../components/Tasks.vue";
import AddTask from "../components/AddTask.vue";
export default {
  name: "Home",
  props: {
    showAddTask: {
      type: Boolean,
      default: false,
    },
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    };
  },
  methods: {
    async addTask(task) {
      const response = await fetch("http://localhost:3000/tasks", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify(task),
      });

      const data = await response.json();

      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id) {
      if (confirm("Are you sure you want to delete this task?")) {
        const response = await fetch(`http://localhost:3000/tasks/${id}`, {
          method: "DELETE",
        });

        response.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Something went wrong");
      }
    },
    async toggleReminder(id) {
      const response = await fetch(`http://localhost:3000/tasks/${id}`, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({
          reminder: !this.tasks.find((task) => task.id === id).reminder,
        }),
      });

      this.tasks.find((task) => task.id === id).reminder = !this.tasks.find(
        (task) => task.id === id
      ).reminder;
    },
    async fetchTasks() {
      const response = await fetch("http://localhost:3000/tasks/");
      const data = await response.json();

      return data;
    },
    async fetchTask(id) {
      const response = await fetch(`http://localhost:3000/tasks/${id}`);
      const data = await response.json();

      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>
