<template>
  <div id="app">
    <todoapp/>
  </div>
</template>

<script>
import todoapp from './components/TodoList.vue'

export default {
  name: 'app',
  components: {
    todoapp
  }
}

</script>

<style>

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #05305c;
  margin-top: 60px;
}

</style>
