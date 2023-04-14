<template>
  <div class="sorting__buttons">
    <button :class="['btn__up', readClassButtonUp]" @click="sortingClicked">
      <Arrow :color="readClassButtonUp ? '#FFFFFF' : '#006CFE'" />
    </button>
    <button :class="['btn__down', readClassButtonDown]" @click="sortingClicked">
      <Arrow :color="readClassButtonDown ? '#FFFFFF' : '#006CFE'" />
    </button>
  </div>
</template>

<script>
import Arrow from "@/components/icons/Arrow.vue";
export default {
  computed: {
    readClassButtonUp() {
      const query = this.$route.query.sorting
      if (query && query.indexOf('-') === -1) {
        return 'active';
      }
    },
    readClassButtonDown() {
      const query = this.$route.query.sorting
      if (query && query.indexOf('-') === 0) {
        return 'active';
      }
    },
  },
  methods: {
    sortingClicked() {
      const query = this.$route.query.sorting
      if (query) {
        this.$emit('sorting', query)

      }
    },
  },
  components: {
    Arrow,
  },
  emits: ['sorting'],
}
</script>

<style lang="scss">
.sorting__buttons {
  display: flex;
  align-items: center;
  gap: 5px;

  button {
    border-radius: 4px;
    height: 44px;
    width: 44px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: transparent;
    border: 1px solid $blue;
  }

  .btn__up {
    svg {
      transform: rotateZ(180deg);
    }
  }

  button.active {
    background-color: $blue;
  }
}
</style>