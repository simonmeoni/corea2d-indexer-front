<template>
  <div id="app">
    <api-viewer/>
  </div>
</template>

<script>
import ApiViewer from './components/ApiViewer';

export default {
  name: 'app',
  components: {
    ApiViewer,
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
