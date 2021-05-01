<template>
  <input
    class="search"
    placeholder="Напиши что нибудь"
    v-model="searchText"
    @input="onSearch"
  />
</template>

<script>
export default {
  name: "Search",
  data() {
    return {
      searchText: "",
      timeOut: null
    };
  },
  methods: {
    onSearch() {
        clearTimeout(this.timeOut);
        this.timeOut = setTimeout(() => {
            this.$emit("search", this.searchText);
        }, 500);
    },
  },
};
</script>

<style scoped>
.search {
  flex-basis: 70%;
}
</style>