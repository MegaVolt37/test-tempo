<template>
  <div class="search">
    <input :class="['search__input', { 'border-active': isActive }]" placeholder="Поиск" type="text" v-model="searchValue"
      @focus="clickedInput" @blur="unClickedInput">
    <img class="search__img" src="@/assets/images/glass.svg" alt="search glass" width="15" height="16">
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchValue: '',
      isActive: false,
    };
  },
  watch: {
    searchValue(value) {
      this.$emit("search", value)
    },
  },
  methods: {
    clickedInput() {
      this.isActive = true;
    },
    unClickedInput() {
      this.isActive = false;
    },
    mountQueryInput() {
      if (this.$route.query.search) {
        this.searchValue = this.$route.query.search
      }
    },
  },
  mounted() {
    this.mountQueryInput();
  },
  emits: ["search"],
}
</script>

<style lang="scss">
.search {
  position: relative;

  &__input {
    padding: 15px 16px 15px 40px;
    width: 100%;
    border: 1px solid $blue-light;
    border-radius: 4px;
    @extend %text-400-16;
    color: $black;


    &:hover {
      border-color: $blue-middle;
    }

    &::placeholder {
      color: $medium;
    }
  }

  &__input.border-active {
    &:hover {
      border-color: $blue;
    }
  }

  &__img {
    position: absolute;
    left: 15px;
    top: 50%;
    transform: translateY(-50%);
  }
}
</style>