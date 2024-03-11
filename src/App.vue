<template>
  <img alt="Vue logo" src="./assets/logo.png" class="logo">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
  <AddTask/>
  <TaskList/>
</template>

<script>
import AddTask from './components/AddTask.vue'
import TaskList from './components/TaskList.vue'

export default {
  name: 'App',
  components: {
    AddTask,
    TaskList
  }
}
</script>


<style>
*{
  box-sizing: border-box;
}
.container{
  max-width: 600px;
  margin: 0 auto;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.logo{
  display:block;
  margin:20px auto;
  height:75px;
}
</style>
