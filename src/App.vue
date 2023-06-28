<template>
  <div class="app">
    <h1>To-Do List</h1>
    <TodoList />
  </div>
</template>

<script setup>
import TodoList from './components/TodoList.vue';
</script>

<style>
.app {
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}
</style>
