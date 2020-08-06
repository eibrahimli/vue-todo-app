<template>
  <div id="app" class="app">
    <img alt="Vue logo" src="./assets/logo.png" class="logo">
    <todo-list></todo-list>
  </div>
</template>

<script>
import TodoList from './components/TodoList'

export default {
  name: 'App',
  components: {
    TodoList
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

.app{
  max-width: 600px;
  margin: 20px auto;
}
.logo{
  display: block;
  margin: 20px auto;
  height: 75px;
}
</style>
