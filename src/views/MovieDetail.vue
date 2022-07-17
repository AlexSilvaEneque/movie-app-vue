<script setup>
    import { ref, onBeforeMount } from "vue"
    import { useRoute } from "vue-router";
    import env from "../env"

    const movie = ref({})
    const route = useRoute()

    onBeforeMount( async() => {
        const pet = await fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&i=${route.params.id}&plot=full`)
        const data = await pet.json()
        movie.value = data
    })
    
</script>

<template>
    <div class="movie-detail">
        <h2>{{ movie.Title }}</h2>
        <p>{{ movie.Year }}</p>
        <img :src="movie.Poster" alt="Movie Poster" class="featured-img">
        <p>{{ movie.Plot }}</p>
    </div>
</template>

<style lang="scss" scoped>
    .movie-detail {
        padding: 16px;

        h2 {
            color: #fff;
            font-size: 28px;
            font-weight: 600;
            margin-bottom: 16px;
        }

        .featured-img {
            display: block;
            max-width: 200px;
            margin-bottom: 16px;
        }

        p {
            color: #fff;
            font-size: 18px;
            line-height: 1.4;
        }
    }
</style>