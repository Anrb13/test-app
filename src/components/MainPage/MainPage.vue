<template>
  <div class="main-page">
    <header class="main-page__title">
      <h1 class="main-page__title-text">
        Список пользователей
      </h1>
      <button
        class="btn waves-effect waves-light"
        :class="{'disabled': loading}"
        @click="showDialog"
        tabindex="0"
      >
        Добавить
      </button>
    </header>

    <div
      class="main-page__table"
    >
      <div class="main-page__header">
        <div
          class="main-page__table-id main-page__header-item"
          @click="sortByType('id')"
          @keypress.enter="sortByType('id')"
          tabindex="0"
        >
          <div>ID</div>
          <TaSortIcon
            :active="isSortById"
            :reverse="isSortById && sortReverse"
            class="main-page__sort"
          />
        </div>
        <div
          class="main-page__table-lastname main-page__header-item"
          @click="sortByType('ln')"
          @keypress.enter="sortByType('ln')"
          tabindex="0"
        >
          <div>Фамилия</div>
          <TaSortIcon
            :active="isSortByLastName"
            :reverse="isSortByLastName && sortReverse"
            class="main-page__sort"
          />
        </div>
        <div
          class="main-page__table-firstname main-page__header-item"
          @click="sortByType('fn')"
          @keypress.enter="sortByType('fn')"
          tabindex="0"
        >
          <div>Имя</div>
          <TaSortIcon
            :active="isSortByFirstName"
            :reverse="isSortByFirstName && sortReverse"
            class="main-page__sort"
          />
        </div>
        <div
          class="main-page__table-birthday"
          @click="sortByType('bd')"
          @keypress.enter="sortByType('bd')"
          tabindex="0"
        >
          <div>Дата рождения</div>
        </div>
      </div>

      <div
        class="main-page__table-body"
        :class="{'main-page__table-body--loading': loading}"
      >
        <div
          v-for="(user, index) in modelUsers"
          class="main-page__table-body-item"
          :key="`${user.id}_${index}`"
        >
          <span class="main-page__table-id">{{ user.id }}</span>
          <span class="main-page__table-lastname">{{ user.lastName }}</span>
          <span class="main-page__table-firstname">{{ user.firstName }}</span>
          <span class="main-page__table-birthday">{{ user.birthday }}</span>
        </div>
      </div>

      <TaLoader v-if="loading" class="main-page__loader" />
    </div>

    <div class="main-page__pagination">
      <button
        class="btn-small"
        :class="{'disabled': users.length <= endAt || loading}"
        type="button"
        tabindex="0"
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
import TaSortIcon from '@/components/TaSortIcon';
import { initialUsers } from './constants';

export default {
  name: 'MainPage',
  components: {
    TaLoader,
    TaDialog,
    TaSortIcon,
  },

  data() {
    return {
      users: [{}, {}, {}, {}, {}],
      modelUsers: [{}, {}, {}, {}, {}],
      startAt: 0,
      endAt: 5,
      loading: true,
      dialogShown: false,
      // Сортировка
      sortType: 'id',
      sortReverse: false,
    };
  },

  watch: {
    users: {
      handler() {
        this.fillModel();
      },
      immediate: true,
    },
    endAt() {
      this.fillModel();
    },
    sortType() {
      this.sortModel();
    },
    sortReverse() {
      this.sortModel();
    },
  },

  computed: {
    isSortById() {
      const { sortType } = this;
      return sortType === 'id';
    },
    isSortByLastName() {
      const { sortType } = this;
      return sortType === 'ln';
    },
    isSortByFirstName() {
      const { sortType } = this;
      return sortType === 'fn';
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
        this.endAt += 5;
        this.loading = false;
      }, 500);
    },
    sortByType(type) {
      if (this.sortType === type) {
        this.sortReverse = !this.sortReverse;
      } else {
        this.sortReverse = false;
        this.sortType = type;
      }
    },
    // Заполняет модель согласно пагинации
    fillModel() {
      const { startAt, endAt } = this;
      this.modelUsers = this.users.slice(startAt, endAt);
    },
    // Сортирует модель соглано типа сортировки
    sortModel() {
      const {
        isSortById, isSortByLastName, isSortByFirstName, sortReverse,
      } = this;

      if (isSortById) {
        if (sortReverse) {
          this.modelUsers.sort((a, b) => +b.id - +a.id);
        } else {
          this.modelUsers.sort((a, b) => +a.id - +b.id);
        }
      }

      if (isSortByLastName) {
        if (sortReverse) {
          this.modelUsers.sort((a, b) => this.getSortValueByChar(b.lastName, a.lastName));
        } else {
          this.modelUsers.sort((a, b) => this.getSortValueByChar(a.lastName, b.lastName));
        }
      }

      if (isSortByFirstName) {
        if (sortReverse) {
          this.modelUsers.sort((a, b) => this.getSortValueByChar(b.firstName, a.firstName));
        } else {
          this.modelUsers.sort((a, b) => this.getSortValueByChar(a.firstName, b.firstName));
        }
      }
    },
    getSortValueByChar(a, b) {
      if (a.charAt(0) < b.charAt(0)) {
        return -1;
      }
      return 1;
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
