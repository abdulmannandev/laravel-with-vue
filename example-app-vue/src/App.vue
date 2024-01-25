<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <h1>Create TODO Task</h1>
  <ToDoFrom @add:task="addTask" v-bind:message="successMessage" />
</template>

<script>
  import ToDoFrom from './components/ToDoFrom.vue'

  export default {
    name: 'App',
    data() {
      return {
        successMessage: ''
      }
    },
    components: {
      ToDoFrom
    },
    methods: {
      async addTask(task) {
        try {
          const response = await fetch('http://127.0.0.1:8000/api/todos', {
            method: 'POST',
            body: JSON.stringify(task),
            headers: { "Content-type": "application/json; charset=UTF-8" }
          });
          const data = await response.json();
          this.successMessage = data.message;
        } catch (error) {
          console.error('Error occured while adding employee: ' +error)
        }
      },
    }
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
