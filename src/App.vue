<template>
  <div id="app">
    <DropdownList :placeholder="placeholder" />
  </div>
</template>

<script>
import DropdownList from './components/DropdownList.vue'

export default {
  name: 'App',
  components: {
    DropdownList,
  },
  data() {
    return {
      placeholder: 'search user',
    }
  },
  methods: {        
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
