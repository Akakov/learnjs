<template>
  <div class="container">
    <h1>Список задач</h1>
    <ul class="task-list">
      <li v-for="task in tasks" :key="task.id" class="task-item">
        <label class="custom-checkbox">
          {{ task.title }}
          <svg v-if="task.done" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"
            class="checkmark-svg">
            <circle cx="12" cy="12" r="10" fill="green" />
            <path fill="none" stroke="white" stroke-width="2" d="M7 12l3 3l7-7" />
          </svg>
          <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" width="24" height="24"
            class="checkmark-svg">
            <circle cx="12" cy="12" r="10" fill="lightgray" />
          </svg>
          <input type="checkbox" v-model="task.done" :id="task.id" @change="saveTasks" />
        </label>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: []
    };
  },
  mounted() {
    const saved = localStorage.getItem("tasks");
    if (saved) {
      this.tasks = JSON.parse(saved);
    } else {
      this.loadTasksFromJSON();
    }
  },
  methods: {
    async loadTasksFromJSON() {
      try {
        const response = await fetch("/tasks.json");
        const data = await response.json();
        this.tasks = data;
        this.saveTasks();
      } catch (error) {
        console.error("Ошибка загрузки задач:", error);
      }
    },


    saveTasks() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    }
  }
};
</script>

<style scoped>
.container {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.task-list {
  list-style-type: none;
  padding: 0;
  background-color: #bccae2;
  border-radius: 8px;
  padding: 20px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.task-item {
  margin: 10px 0;
  font-size: 18px;
}

.custom-checkbox input[type='checkbox'] {
  display: none;
}

.checkmark-svg {
  display: inline-block;
  padding: 0px 0px 0px 10px;
  transition: all 0.3s ease;
}

label {
  display: flex;
  align-items: center;
  cursor: pointer;
  font-size: 18px;
}
</style>
