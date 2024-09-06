<template>
  <div class="add-todo">
    <input type="text" v-model="newTodo" placeholder="Enter a task" @keyup.enter="addTodo" />
    <button @click="addTodo">Add</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() === '') return;
      this.$emit('add-todo', this.newTodo.trim());
      this.newTodo = '';
    },
  },
};
</script>

<style scoped>
.add-todo {
  display: flex;
  gap: 10px;
}

input {
  flex: 1;
  padding: 8px;
}

button {
  padding: 8px 12px;
  cursor: pointer;
}
</style>