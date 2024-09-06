<template>
    <ul class="todo-list">
      <li v-for="todo in todos" :key="todo.id">
        <label>
          <input
            type="checkbox"
            v-model="todo.completed"
            @change="toggleComplete(todo.id)"
          />
          <span :class="{ completed: todo.completed }">{{ todo.title }}</span>
        </label>
        <button @click="deleteTodo(todo.id)">Delete</button>
      </li>
    </ul>
  </template>
  
  <script>
  export default {
    props: {
      todos: Array,
    },
    methods: {
      toggleComplete(id) {
        this.$emit('toggle-complete', id);
      },
      deleteTodo(id) {
        this.$emit('delete-todo', id);
      },
    },
  };
  </script>
  
  <style scoped>
  .todo-list {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 0;
  }
  .completed {
    text-decoration: line-through;
  }
  button {
    background: red;
    color: white;
    border: none;
    padding: 5px 10px;
    cursor: pointer;
  }
  </style>
  