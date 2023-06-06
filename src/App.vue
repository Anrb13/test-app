<template>
  <div id="app">
    <Home msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import Home from './components/Home.vue';

export default {
  name: 'App',
  components: {
    Home,
  },
};
</script>

<style lang="scss">
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 100%;
  margin: 24px 14px;

  @media (min-width: 768px) {
    width: 768px;
    margin: auto;
  }
}

@import '~materialize-css/dist/css/materialize.min.css';
@import '@/assets/reset.scss';
</style>
