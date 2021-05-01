<template>
  <div id="app">
    <div class="header">
      <div class="logo">GIPHY</div>
      <Search @search="search" />
    </div>
    <GifList v-if="gifs.length > 0" :gifs="gifs" />
    <h2 v-else class="empty-text">{{ emptyText }}</h2>
  </div>
</template>

<script>
import Search from "./components/Search.vue";
import GifList from "./components/GifList.vue";

export default {
  name: "App",
  components: {
    Search,
    GifList,
  },
  data() {
    return {
      emptyText: "Loading...",
      gifs: [],
      gifsOffset: 0,
      isSearch: false,
      searchText: "",
    };
  },
  created() {
    this.trendGifs("start");
    window.addEventListener("scroll", this.handleScroll);
  },
  destroyed() {
    window.removeEventListener("scroll", this.handleScroll);
  },
  methods: {
    handleScroll() {
      if (window.scrollY + window.innerHeight >= document.body.scrollHeight) {       
        if (this.isSearch) {
         this.searchGifs("load");
        } else {
          this.trendGifs("load");
        }
      }
    },
    search(searchText) {
      if (searchText !== "") {
        this.emptyText = "Not Found"
        this.searchText = searchText;
        this.isSearch = true;

        this.searchGifs("search");
      } else {
        this.isSearch = false;
        this.trendGifs("start");
      }
    },
    async searchGifs(mode) {
      try {
        this.gifsOffset = mode === "search" ? 0 : this.gifsOffset += 20;
        
        await fetch(`https:api.giphy.com/v1/gifs/search?api_key=${process.env.VUE_APP_GIPHY_KEY}&q=${this.searchText}&offset=${ this.gifsOffset }&limit=20`)
        .then((res) => res.json())
        .then((result) => {
          if (mode === "search") {
            this.gifs = result.data;
          } 
          else if (mode === "load") {
            this.gifs.push(...result.data);
          }
        });
      } catch (e) {}
    },
    async trendGifs(mode) {
      try {
        this.gifsOffset = mode === "start" ? 0 : this.gifsOffset += 20;

        await fetch(`https:api.giphy.com/v1/gifs/trending?api_key=${process.env.VUE_APP_GIPHY_KEY}&offset=${ this.gifsOffset }&limit=20`)
        .then((res) => res.json())
        .then((result) => {
          if (mode === "start") {
            this.gifs = result.data;
          } 
          else if (mode === "load") {
            this.gifs.push(...result.data);
          }
        });
      } catch (e) {}
    },
  },
};
</script>

<style>
body {
  background: #000;
  font-family: "Courier New", Courier, monospace;
}

.header {
  display: flex;
  justify-content: space-between;
  padding: 10px 5px 0;
  margin-bottom: 20px;
}

.logo {
  color: #ffeb3b;
  font-family: cursive;
  font-size: 20px;
}

.empty-text {
  color: gainsboro;
  text-align: center;
  margin-top: 50px;
}
</style>