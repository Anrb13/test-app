<!-- eslint-disable vuejs-accessibility/label-has-for -->
<template>
  <form class="ta-dialog z-depth-3" data-test="add-new-user-form">
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
        :class="{'invalid': error.lastName}"
        maxlength="30"
        v-model="model.lastName"
        @input="error.lastName = false"
      >
      <span v-if="error.lastName" class="ta-dialog__error">
        Недопустимая фамилия
      </span>
    </label>
    <label for="f-name" class="ta-dialog__label">
      Имя
      <input
        id="f-name"
        type="text"
        class="ta-dialog__input"
        :class="{'invalid': error.firstName}"
        maxlength="30"
        v-model="model.firstName"
        @input="error.firstName = false"
      >
      <span v-if="error.firstName" class="ta-dialog__error">
        Недопустимое имя
      </span>
    </label>
    <label for="date" class="ta-dialog__label">
      Дата рождения
      <input
        id="date"
        type="date"
        class="ta-dialog__input"
        :class="{'invalid': error.birthday}"
        v-model="model.birthday"
        @input="error.birthday = false"
      >
      <span v-if="error.birthday" class="ta-dialog__error">
        Недопустимое значение даты
      </span>
    </label>

    <button
      class="ta-dialog__submit btn waves-effect waves-light"
      type="button"
      name="action"
      data-test="add-new-user-form-btn-submit"
      @click="submit"
    >
      Добавить
      <i class="material-icons right">send</i>
    </button>

  </form>
</template>

<script>
export default {
  name: 'TaDialog',

  data() {
    return {
      model: {
        lastName: '',
        firstName: '',
        birthday: '',
      },
      error: {
        lastName: false,
        firstName: false,
        birthday: false,
      },
    };
  },

  computed: {
    isLastNameValid() {
      const { lastName } = this.model;
      return lastName.length > 1 && lastName.length < 30 && (/^[А-Яа-я ,.'-]+$/i).test(lastName);
    },
    isFirstNameValid() {
      const { firstName } = this.model;
      return firstName.length > 1 && firstName.length < 30 && (/^[А-Яа-я ,.'-]+$/i).test(firstName);
    },
    isBirthdayValid() {
      const { birthday } = this.model;
      return birthday.length === 10;
    },
    isFormValid() {
      const { isLastNameValid, isFirstNameValid, isBirthdayValid } = this;
      return isLastNameValid && isFirstNameValid && isBirthdayValid;
    },
  },

  methods: {
    submit() {
      const {
        model, isFormValid, isLastNameValid, isFirstNameValid, isBirthdayValid,
      } = this;

      if (isFormValid) {
        this.$emit('submit', model);
      } else {
        if (!isLastNameValid) {
          this.error.lastName = true;
        }
        if (!isFirstNameValid) {
          this.error.firstName = true;
        }
        if (!isBirthdayValid) {
          this.error.birthday = true;
        }
      }
    },
  },
};
</script>

<style lang="scss">
@import './styles.scss';
</style>
