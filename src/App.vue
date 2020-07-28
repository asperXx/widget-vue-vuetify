<template>
  <div id="app">
    <v-app>
    <Widget />
    </v-app>
  </div>
</template>

<script>
import Widget from './components/Widget.vue'

export default {
  name: 'App',
  components: {
    Widget
  }
}
</script>

<style lang="scss">
#app {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #2c3e50;
  margin-top: 3px;
}
</style>
