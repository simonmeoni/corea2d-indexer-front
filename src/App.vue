<template>
  <div id="app">
    <api-viewer class="section is-size-7-mobile"></api-viewer>
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
