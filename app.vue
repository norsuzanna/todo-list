<template>
  <div id="app">
    <h1>To-Do List</h1>
    <AddTodo @add-todo="addTodo" />
    <TodoList
      :todos="todos"
      @toggle-complete="toggleComplete"
      @delete-todo="deleteTodo"
    />
  </div>
</template>

<script>
import axios from 'axios';
import AddTodo from './components/AddTodo.vue';
import TodoList from './components/TodoList.vue';

export default {
  components: {
    AddTodo,
    TodoList,
  },
  data() {
    return {
      todos: [],
    };
  },
  mounted() {
    // Fetch existing tasks from the backend when the component is mounted
    this.fetchTodos();
  },
  methods: {
    // Fetch tasks from the backend
    async fetchTodos() {
      try {
        const response = await axios.get('https://m8vqll-8000.csb.app/todos');
        this.todos = response.data;
      } catch (error) {
        console.error('Error fetching todos:', error);
      }
    },

    // Add a new task by sending a POST request to the backend
    async addTodo(title, description) {
      const newTodo = {
        id: Date.now(), // Or leave it out and let the backend handle it
        title: title,
        description: description,
        completed: false,
      };
      try {
        const response = await axios.post('https://m8vqll-8000.csb.app/todos', newTodo);
        this.todos.push(response.data);
        this.fetchTodos();
      } catch (error) {
        console.error('Error adding todo:', error);
      }
    },

    // Toggle the completion status of a task by sending a PUT request
    async toggleComplete(id) {
      const todo = this.todos.find((todo) => todo.id === id);
      if (todo) {
        const updatedTodo = {
          ...todo,
          completed: !todo.completed,
        };
        try {
          const response = await axios.put(`https://m8vqll-8000.csb.app/${id}`, updatedTodo);
          todo.completed = response.data.completed;
        } catch (error) {
          console.error('Error updating todo:', error);
        }
      }
    },

    // Delete a task by sending a DELETE request
    async deleteTodo(id) {
      try {
        await axios.delete(`https://m8vqll-8000.csb.app/${id}`);
        this.todos = this.todos.filter((todo) => todo.id !== id);
      } catch (error) {
        console.error('Error deleting todo:', error);
      }
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin-top: 20px;
}
h1 {
  color: #42b983;
}
</style>
