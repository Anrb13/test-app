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
        <tr v-for="user in slicedUsers" :key="user.id">
          <th>{{ user.id }}</th>
          <th>{{ user.lastName }}</th>
          <th>{{ user.firstName }}</th>
          <th>{{ user.birthday }}</th>
        </tr>
      </tbody>

      <TaLoader v-if="loading" class="main-page__loader" />
    </table>

    <div class="main-page__pagination">
      <button
        class="btn-small"
        :class="{'disabled': users.length <= pagination.endAt || loading}"
        type="button"
        @click="paginationPlus"
      >
        Загрузить ещё
      </button>
    </div>

    <TaDialog
      v-if="dialogShown"
      class="main-page__dialog"
      @close="closeDialog"
      @submit="addNewUser"
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
      pagination: {
        startAt: 0,
        endAt: 5,
      },
      loading: true,
      dialogShown: false,
    };
  },

  computed: {
    slicedUsers() {
      const { startAt, endAt } = this.pagination;
      return this.users.slice(startAt, endAt);
    },
  },

  methods: {
    /**
     * При монтировании компонента проверяет локал сторейдж
     * на наличие уже записанного массива users, если нет -
     * заполняет, с таймаутом для лоадера записывает в дату
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
    addNewUser(value) {
      this.loading = true;
      this.closeDialog();

      setTimeout(() => {
        this.users.push({ id: this.users.length + 1, ...value });
      }, 500);

      setTimeout(() => {
        localStorage.setItem('users', JSON.stringify(this.users));
        this.loading = false;
      }, 750);
    },
    paginationPlus() {
      this.loading = true;

      setTimeout(() => {
        this.pagination.endAt += 5;
        this.loading = false;
      }, 500);
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
