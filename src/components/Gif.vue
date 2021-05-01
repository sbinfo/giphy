<template>
  <div class="gif">
    <img :src="gif.images.original.url" alt="">
  </div>
</template>

<script>
export default {
  name: "Gif",
  props: {
    gif: Object
  }
};
</script>

<style scoped>
.gif {
  flex-basis: 49%;
  margin-bottom: 15px;
  height: auto;
}

.gif img {
    width: 100%;
    height: 150px;
    object-fit: cover;
}

@media (min-width: 768px) {
  .gif {
    flex-basis: 19%;
    margin-bottom: 15px;
  }
  .gif img {
    height: 200px;
  }
}
</style>