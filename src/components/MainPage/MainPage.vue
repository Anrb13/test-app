<template>
  <div class="main-page">
    <header class="main-page__header">
      <h1 class="main-page__header-title">
        Список пользователей
      </h1>
      <button
        class="btn main-page__header-button waves-effect waves-light"
        :class="{'disabled': loading}"
        @click="showDialog"
      >
        Добавить
      </button>
    </header>

    <table
      class="main-page__table striped"
    >
      <thead class="main-page__table-head">
        <tr>
          <th>ID</th>
          <th>Фамилия</th>
          <th>Имя</th>
          <th>Дата рождения</th>
        </tr>
      </thead>
      <tbody
        class="main-page__table-body"
        :class="{'main-page__table-body--loading': loading}"
      >
        <tr v-for="user in users" :key="user.id">
          <th>{{ user.id }}</th>
          <th>{{ user.lastName }}</th>
          <th>{{ user.firstName }}</th>
          <th>{{ user.birthday }}</th>
        </tr>
      </tbody>

      <TaLoader v-if="loading" class="main-page__loader" />
    </table>
    <TaDialog
      v-if="dialogShown"
      class="main-page__dialog"
      @close="closeDialog"
    />
  </div>
</template>

<script>
import TaLoader from '@/components/TaLoader';
import TaDialog from '@/components/TaDialog';
import { initialUsers } from './constants';

export default {
  name: 'MainPage',
  components: {
    TaLoader,
    TaDialog,
  },

  data() {
    return {
      users: [{}, {}, {}, {}, {}, {}],
      // Число показанных строк
      count: 5,
      loading: true,
      dialogShown: false,
    };
  },

  computed: {
    slicedUsers() {
      return this.users.slice(0, this.count);
    },
  },

  methods: {
    /**
     * При монтировании компонента проверяем локал сторейдж
     * на наличие уже записанного массива users, если нет
     * заполняем, с таймаутом для лоадера записывает в дату
     */
    init() {
      if (!localStorage.getItem('users')) {
        localStorage.setItem('users', JSON.stringify(initialUsers));
      }

      setTimeout(() => {
        this.users = JSON.parse(localStorage.getItem('users'));
        this.loading = false;
      }, 2000);
    },
    showDialog() {
      this.dialogShown = true;
    },
    closeDialog() {
      this.dialogShown = false;
    },
  },

  mounted() {
    this.init();
  },
};
</script>

<style lang="scss">
@import './styles.scss';
</style>
