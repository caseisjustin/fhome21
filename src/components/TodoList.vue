<template>
    <div>
      <slot name="header"></slot> 

      <input v-model="newTask" @keyup.enter="addTask" placeholder="Add a new task" />

      <transition-group name="list" tag="ul">
        <li v-for="(task, index) in tasks" :key="task.name">
          <span :style="{ textDecoration: task.completed ? 'line-through' : 'none' }">{{ task.name }}</span>
          <slot name="task-buttons" :index="index"></slot>
        </li>
      </transition-group>
  
      <slot name="footer"></slot>
    </div>
  </template>
  
  <script>
  export default {
    props: ['tasks'],
    data() {
      return {
        newTask: '',
      };
    },
    methods: {
      addTask() {
        if (this.newTask.trim() !== '') {
          this.tasks.push({ name: this.newTask, completed: false });
          this.newTask = '';
        }
      },
    },
  };
  </script>
  
  <style>
  .list-enter-active, .list-leave-active {
    transition: all 0.5s;
  }
  .list-enter, .list-leave-to {
    opacity: 0;
    transform: translateY(30px);
  }
  </style>
  