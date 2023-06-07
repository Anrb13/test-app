<template>
  <div class="main-page">
    <header class="main-page__header">
      <h1 class="main-page__header-title">Список пользователей</h1>
      <button class="btn main-page__header-button light-green">Добавить</button>
    </header>

    <table
      class="main-page__table striped"
      :class="{'main-page__table--loading': loading}"
    >
      <thead class="main-page__table-head">
        <tr>
          <th>ID</th>
          <th>Фамилия</th>
          <th>Имя</th>
          <th>Дата рождения</th>
        </tr>
      </thead>
      <tbody class="main-page__table-body">
        <tr v-for="user in users" :key="user.id">
          <th>{{ user.id }}</th>
          <th>{{ user.lastName }}</th>
          <th>{{ user.firstName }}</th>
          <th>{{ user.birthday }}</th>
        </tr>
      </tbody>

      <div v-if="loading" class="main-page__loader">
        <div class="preloader-wrapper big active">
          <div class="spinner-layer spinner-blue-only">
            <div class="circle-clipper left">
              <div class="circle"></div>
            </div><div class="gap-patch">
              <div class="circle"></div>
            </div><div class="circle-clipper right">
              <div class="circle"></div>
            </div>
          </div>
        </div>
      </div>
    </table>
  </div>
</template>

<script>
const initialUsers = [
  {
    id: '1',
    lastName: 'Яблонев',
    firstName: 'Андрей',
    birthday: '01.01.1960',
  },
  {
    id: '2',
    lastName: 'Юдин',
    firstName: 'Владимир',
    birthday: '01.01.2000',
  },
  {
    id: '3',
    lastName: 'Экономов',
    firstName: 'Борис',
    birthday: '01.01.1980',
  },
  {
    id: '4',
    lastName: 'Головкин',
    firstName: 'Геннадий',
    birthday: '01.01.1940',
  },
];

export default {
  name: 'MainPage',

  data() {
    return {
      users: [{}, {}, {}, {}, {}, {}],
      // Число показанных строк
      count: 5,
      loading: true,
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
  },

  mounted() {
    this.init();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import './styles.scss';
</style>
