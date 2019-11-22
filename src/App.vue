<template>
  <div
    id="app"
    class="container mx-auto flex flex-wrap justify-center font-sans text-gray-800"
  >
    <img
      alt="Vue logo"
      src="./assets/logo.png"
      class=""
    >
    <todo-list class="w-full mt-4"></todo-list>
  </div>
</template>

<script>
import './assets/css/tailwind.css';
import TodoList from './components/ToDoList.vue';

export default {
  name: 'app',
  components: {
    TodoList,
  },
};
</script>
