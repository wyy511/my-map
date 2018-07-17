<template lang="pug">
  #app
    router-view
    
</template>

<script>

export default {
  name: 'App',
  data () {
    return {

    }
  },
  
  methods: {
    drag (data) {
      console.log(data)
    }
  }
}
</script>

<style>
html, body {
  width: 100%;
  height: 100%;
  overflow: hidden;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  width: 100%;
  height: 100%;
}
</style>
