<template>
  <div class="qa-home">
    <header class="qa-home__header">
      <h1 class="qa-home__header-title">Список пользователей</h1>
      <button class="btn qa-home__header-button">Добавить</button>
    </header>
  </div>
</template>

<script>
export default {
  name: 'QaHome',
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import './styles.scss';
</style>
