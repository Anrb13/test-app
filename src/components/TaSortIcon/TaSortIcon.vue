<template>
  <div
    class="ta-sort-icon"
    :class="{'ta-sort-icon--active': active}"
  >
    <i class="material-icons">sort</i>
    <i
      class="material-icons dropdown"
      :class="{'reverse': reverse}"
    >
      arrow_drop_down
    </i>
  </div>
</template>

<script>
export default {
  name: 'TaSortIcon',

  props: {
    active: {
      type: Boolean,
      default: false,
    },
    reverse: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style lang="scss">
@import './styles.scss';
</style>
