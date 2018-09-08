<template>
  <div id="app">
    <Saying />
  </div>
</template>

<script>
import Saying from '@/components/saying.vue';
export default {
  name: 'App',
  components: {
    Saying
  },
  created () {

  }
}
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#app {
  margin: 0;
  padding: 0;
}
</style>
