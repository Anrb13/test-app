<!-- eslint-disable vuejs-accessibility/label-has-for -->
<template>
  <form class="ta-dialog z-depth-3">
    <h2 class="ta-dialog__title">
      Добавление нового пользователя
      <button
        class="ta-dialog__close btn-floating btn-small red"
        type="button"
        @click="$emit('close')"
      >
        <i class="material-icons">close</i>
      </button>
    </h2>

    <label for="l-name" class="ta-dialog__label">
      Фамилия
      <input
        id="l-name"
        type="text"
        class="ta-dialog__input"
        minlength="2"
        maxlength="30"
      >
    </label>
    <label for="f-name" class="ta-dialog__label">
      Имя
      <input
        id="f-name"
        type="text"
        class="ta-dialog__input"
        minlength="2"
        maxlength="30"
      >
    </label>
    <label for="date" class="ta-dialog__label">
      Дата рождения
      <input id="date" type="date" class="ta-dialog__input">
    </label>

    <button class="ta-dialog__submit btn waves-effect waves-light" type="button" name="action">
      Добавить
      <i class="material-icons right">send</i>
    </button>

  </form>
</template>

<script>
export default {
  name: 'TaDialog',
};
</script>

<style lang="scss">
@import './styles.scss';
</style>
