<template>
  <div>
    <h1>Main Todo App</h1>
    
    <!-- Using keep-alive for the AdditionalComponent -->
    <keep-alive>
      <AdditionalComponent v-if="showAdditional" />
    </keep-alive>
    <button @click="toggleAdditional">Toggle Additional Info Component</button>

    <!-- Asynchronous Todo List component with slots -->
    <AsyncTodoList :tasks="tasks">
      <!-- Slot for Custom Header -->
      <template #header>
        <h3>Custom Header for Todo List</h3>
      </template>

      <!-- Slot for Task Buttons with transitions -->
      <template #task-buttons="{ index }">
        <transition name="fade">
          <span v-if="tasks[index]">
            <button @click="toggleComplete(index)">✔</button>
            <button @click="editTask(index)">✎</button>
            <button @click="deleteTask(index)">🗑</button>
          </span>
        </transition>
      </template>

      <!-- Slot for Custom Footer -->
      <template #footer>
        <p>Custom Footer: Manage your tasks efficiently!</p>
      </template>
    </AsyncTodoList>
  </div>
</template>

<script>
import { defineAsyncComponent } from 'vue';

export default {
  data() {
    return {
      tasks: [
        { name: 'Task 1', completed: false },
        { name: 'Task 2', completed: true },
      ],
      showAdditional: false,
    };
  },
  components: {
    // Async component loading
    AsyncTodoList: defineAsyncComponent(() => import('./components/TodoList.vue')),
    AdditionalComponent: defineAsyncComponent(() => import('./components/AdditionalComponent.vue')),
  },
  methods: {
    toggleComplete(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
    },
    editTask(index) {
      const editedTask = prompt('Edit task', this.tasks[index].name);
      if (editedTask) this.tasks[index].name = editedTask;
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    toggleAdditional() {
      this.showAdditional = !this.showAdditional;
    },
  },
};
</script>

<style scoped>
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
