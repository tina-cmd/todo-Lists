<template>
    <div id="app">
      <TodoList />
    </div>
  </template>
  
  <script>
  import TodoList from './components/todoList.vue'; // Importing the TodoList component
  
  export default {
    name: 'App',
    components: {
      TodoList, // Registering the component
    },
  };
  </script>
  
  <style scoped>
  #app {
    text-align: center;
    margin-top: 20px;
  }
  </style>
  