<template>
  <div class="select">
    <div :class="['select__top', { 'border-active': isActive }]" @click="openList">
      <span :class="['select__top-placeholder', { 'small': select }]">Сортировать по</span>
      <p class="select__top-title">{{ select }}</p>
      <IconChevron :style="styleArrow" />
    </div>
    <ul class="select__list" v-if="isActive">
      <li class="select__list-item" v-for="(item, index) in list" :key="item.title" @click="selectItem(item, index)">{{
        item.title }}</li>
    </ul>
  </div>
</template>

<script>
import IconChevron from "@/components/icons/Shevron.vue"
export default {
  data() {
    return {
      isActive: false,
      select: '',
    };
  },
  computed: {
    styleArrow() {
      return this.isActive ? 'transform: rotateZ(180deg);' : 'transform: rotateZ(0deg);'
    },
  },
  methods: {
    openList() {
      this.isActive = !this.isActive;
    },
    selectItem(item, index) {
      this.isActive = false;
      if (item.title === this.select) return
      this.select = item.title;
      this.$emit('selectItem', item.id, index)
    },
    mountSelect() {
      const query = this.$route.query.sorting
      const res = this.list.find(el => el.id == query)
      this.select = res ? res?.title : '';
    },

  },
  mounted() {
    this.mountSelect()
  },
  props: {
    list: Array,
  },
  components: {
    IconChevron,
  },
  emits: ['selectItem'],
}
</script>

<style lang="scss">
.select {
  position: relative;
  flex: 1;

  &__top {
    display: grid;
    grid-template-columns: 1fr auto;
    align-items: center;
    background-color: $white;
    border-radius: 4px;
    overflow: hidden;
    padding: 7px 10px;
    border: 1px solid $blue-light;

    &-placeholder {
      grid-row: 1;
      @extend %text-400-14;
      color: $medium;
    }

    &-placeholder.small {
      @extend %text-400-11;
    }

    &-title {
      grid-row: 2;
      @extend %text-400-14;
    }



    svg {
      grid-row: 1/3;
      // grid-column: 2;
    }
  }

  &__top.border-active {}

  &__list {
    z-index: 1;
    position: absolute;
    top: 115%;
    left: 0;
    right: 0;
    border: 1px solid $blue-light;
    border-radius: 4px;
    overflow: hidden;
    display: grid;

    &-item {
      padding: 9px 10px;
      background-color: $white;
      border-bottom: 1px solid $blue-light;
      @extend %text-400-14;
    }

    &-item:hover {
      background-color: $blue-super-light;

    }

    &-item:last-child {
      border-bottom: none;

    }
  }
}
</style>