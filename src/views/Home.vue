<template>
  <AddTask @add-task="addTask" v-if="showAddTask"></AddTask>

  <Tasks
    @toggle-reminder="toggleReminder"
    :tasks="tasks"
    @delete-task="deleteTask"
  ></Tasks>
</template>

<script>
import Tasks from "@/components/Tasks";
import AddTask from "@/components/AddTask";

export default {
  name: "Home",
  props: {
    showAddTask: Boolean,
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
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(task),
      });
      const data = await res.json();

      this.tasks = [...this.tasks, data];
    },
    async deleteTask(id) {
      if (confirm("Are you sure?")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });

        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert("Error deleting task");
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updTask),
      });

      const data = await res.json();

      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      );
    },
    async fetchTasks() {
      try {
        const res = await fetch("api/tasks");
        const data = await res.json();
        return data;
      } catch (error) {
        console.error(error);
      }
    },
    async fetchTask(id) {
      try {
        const res = await fetch(`api/tasks/${id}`);
        const data = await res.json();
        return data;
      } catch (error) {
        console.error(error);
      }
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
  },
};
</script>
