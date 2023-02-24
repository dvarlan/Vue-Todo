<template>
  <div id="App">
    <Todo />
  </div>
</template>

<script>
import Todo from './components/Todo.vue'

export default {
  name: 'App',
  components: {
    Todo
  }
}
</script>

<style>
html {
  background-color: #272727;
  color: #e7e9ea;
  font-family: 'Red Hat Mono', sans-serif;
}
#App {
  width: max-content;
  margin-left: auto;
  margin-right: auto;
}
</style>
