<template>
  <div id="app">
    <MainPage />
  </div>
</template>

<script>
import MainPage from './components/MainPage';

export default {
  name: 'App',
  components: {
    MainPage,
  },
};
</script>

<style lang="scss">
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 100%;
  margin: 24px 14px;

  @media (min-width: 600px) {
    width: 600px;
    margin: auto;
  }
}

@import '~materialize-css/dist/css/materialize.min.css';
@import '@/assets/reset.scss';
</style>
