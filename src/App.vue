<template>
  <div class="container pt-4">
   <SistemaGastos/>
  </div>
</template>

<script>
import SistemaGastos from './components/SistemaGastos.vue'


export default {
  name: 'App',
  components: {
    SistemaGastos

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
