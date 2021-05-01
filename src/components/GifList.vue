<template>
    <div class="gif-list">
       <Gif 
        class="gif"
        v-for="gif in gifs"
        :key="gif.id"
        :gif="gif"
        />
    </div>
</template>

<script>
import Gif from "./Gif.vue"

export default {
    name: 'GifList',
    components: { Gif },
    props: {
        gifs: Array
    }
}
</script>

<style scoped>
.gif-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
}
</style>