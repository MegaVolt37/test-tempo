<template>
  <div class="section">
    <div class="container">
      <h1 class="title__page">Список заявлений</h1>
      <Search @search="search" />
      <Table />
    </div>

  </div>
</template>

<script>
import Search from "@/components/Search.vue";
import Table from "@/components/Table.vue";
export default {
  data() {
    return {
    };
  },
  methods: {
    search(value) {
      if (value) {
        this.$router.push({ query: { ...this.$route.query, search: value } });
      } else {
        const { search, ...query } = this.$route.query;
        this.$router.replace({ query });
      }
    },
  },
  components: { Search, Table }
}
</script>

<style lang="scss" scoped>
.title__page {
  margin-bottom: 20px;
}
</style>