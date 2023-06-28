<template>
    <div class="todo-list">
      <h2 class="title">To-Do List</h2>
      <div class="input-container">
        <input v-model="newTodo" placeholder="Add a new task" @keydown.enter="addTodo" />
        <button class="add-btn" @click="addTodo">Add</button>
      </div>
      <ul>
        <li v-for="(todo, index) in todos" :key="index" class="todo-item">
          <input type="checkbox" v-model="todo.completed" class="checkbox" />
          <span :class="{ completed: todo.completed }">{{ todo.title }}</span>
          <div class="btn-container">
            <button @click="deleteTodo(index)" class="delete-btn">Delete</button>
            <button @click="undoDelete(index)" v-if="deletedTodos[index]" class="undo-btn">Undo</button>
          </div>
        </li>
      </ul>
    </div>
  </template>
  
  <script setup>
  import { ref, onMounted, watch } from 'vue';
  
  const todos = ref([]);
  const newTodo = ref('');
  const deletedTodos = ref([]);
  
  const addTodo = () => {
    if (newTodo.value.trim() !== '') {
      todos.value.push({ title: newTodo.value, completed: false });
      newTodo.value = '';
    }
  };
  
  const deleteTodo = (index) => {
    const deletedTodo = todos.value.splice(index, 1);
    deletedTodos.value[index] = deletedTodo[0];
  };
  
  const undoDelete = (index) => {
    const deletedTodo = deletedTodos.value[index];
    if (deletedTodo) {
      todos.value.splice(index, 0, deletedTodo);
      deletedTodos.value[index] = null;
    }
  };
  
  const saveTodos = () => {
    localStorage.setItem('todos', JSON.stringify(todos.value));
  };
  
  const loadTodos = () => {
    const savedTodos = localStorage.getItem('todos');
    if (savedTodos) {
      todos.value = JSON.parse(savedTodos);
    }
  };
  
  onMounted(() => {
    loadTodos();
  });
  
  watch(todos, () => {
    saveTodos();
  });
  
  // Add an event listener to save todos when the window is unloaded (e.g., when the browser is closed)
  window.addEventListener('beforeunload', saveTodos);
  </script>
  
  <style scoped>
  .todo-list {
    max-width: 500px;
    margin: 0 auto;
    padding: 20px;
  }
  
  .title {
    text-align: center;
    margin-bottom: 20px;
  }
  
  .input-container {
    display: flex;
    margin-bottom: 20px;
  }
  
  .input-container input {
    flex-grow: 1;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  .add-btn {
    padding: 10px 20px;
    background-color: #4caf50;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  .add-btn:hover {
    background-color: #45a049;
  }
  
  ul {
    list-style: none;
    padding: 0;
  }
  
  .todo-item {
    display: flex;
    align-items: center;
    margin-bottom: 10px;
  }
  
  .checkbox {
    margin-right: 10px;
  }
  
  .completed {
    text-decoration: line-through;
    color: #888;
  }
  
  .btn-container {
    margin-left: auto;
  }
  
  .delete-btn,
  .undo-btn {
    padding: 6px 10px;
    background-color: #f44336;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s;
    margin-left: 10px;
  }
  
  .delete-btn:hover,
  .undo-btn:hover {
    background-color: #d32f2f;
  }
  </style>
  