<script setup>
    import { ref } from 'vue';
    import env from "../env";

    const search = ref("")
    const movies = ref([])

    const searchMovie = async () => {
        if (search.value !== "") {
            const pet = await fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
            const data = await pet.json()
            movies.value = data.Search
            search.value = ""
        }
    }

</script>

<template>
    <div class="home">
        <div class="feature-card">
            <router-link to="/movie/tt0409591">
                <img src="https://m.media-amazon.com/images/M/MV5BZmQ5NGFiNWEtMmMyMC00MDdiLTg4YjktOGY5Yzc2MDUxMTE1XkEyXkFqcGdeQXVyNTA4NzY1MzY@._V1_SX300.jpg" alt="Naruto" class="featured-img">
                <div class="detail">
                    <h3>Naruto</h3>
                    <p>Naruto Uzumaki, a mischievous adolescent ninja, struggles as he searches for recognition and dreams of becoming the Hokage, the village's leader and strongest ninja.</p>
                </div>
            </router-link>
        </div>

        <form @submit.prevent="searchMovie" class="search-box">
            <input type="text" placeholder="What are you looking for?" v-model="search">
            <input type="submit" value="Search">
        </form>

        <div class="movie-list">
            <div class="movie" v-for="movie in movies" :key="movie.imdbID">
                <router-link :to="'/movie/'+movie.imdbID" class="movie-link">
                    <div class="product-image">
                        <img :src="movie.Poster" alt="Movie Poster">
                        <div class="type">{{ movie.Type }}</div>
                    </div>
                    <div class="detail">
                        <p class="year">{{ movie.Year }}</p>
                        <h3>{{ movie.Title }}</h3>
                    </div>
                </router-link>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
    .home {
        .feature-card {
            position: relative;

            .featured-img {
                display: block;
                width: 100%;
                height: 300px;
                object-fit: cover;
                position: relative;
                z-index: 0;
            }

            .detail {
                position: absolute;
                left: 0;
                right: 0;
                bottom: 0;
                background-color: rgba($color: #000000, $alpha: 0.6);
                padding: 16px;
                z-index: 1;

                h3 {
                    color: #fff;
                    margin-bottom: 16px;
                }

                p {
                    color: #fff;
                }
            }
        }

        .search-box {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 16px;

            input {
                display: block;
                appearance: none;
                border: none;
                outline: none;
                background: none;

                &[type="text"] {
                    width: 100%;
                    color: #fff;
                    background-color: #496583;
                    font-size: 20px;
                    padding: 10px 16px;
                    border-radius: 8px;
                    margin-bottom: 12px;
                    transition: 0.4s;

                    &::placeholder {
                        color: #f3f3f3;
                    }

                    &:focus {
                        box-shadow: 0px 3px 4px rgba($color: #000000, $alpha: 0.2);
                    }
                }

                &[type="submit"] {
                    width: 100%;
                    max-width: 300px;
                    background-color: #42b893;
                    padding: 16px;
                    border-radius: 8px;
                    color: #fff;
                    font-size: 20px;
                    text-transform: uppercase;
                    transition: 0.4s;

                    &:active {
                        background-color: #3b8070;
                    }
                }
            }
        }

        .movie-list {
            display: flex;
            flex-wrap: wrap;
            margin: 0px 8px;

            .movie {
                max-width: 50%;
                flex: 1 1 50%;
                padding: 16px 8px;

                .movie-link {
                    display: flex;
                    flex-direction: column;
                    height: 100%;

                    .product-image {
                        position: relative;
                        display: block;

                        img {
                            display: block;
                            width: 100%;
                            height: 255px;
                            object-fit: cover;
                        }

                        .type {
                            position: absolute;
                            padding: 8px 16px;
                            background-color: rgba(66, 184, 147, 0.9);
                            border-bottom-right-radius: 5px;
                            border-top-left-radius: 5px;
                            color: #fff;
                            top: 20px;
                            left: 0;
                            text-transform: capitalize;
                        }
                    }
                }

                .detail {
                    background-color: #496583;
                    padding: 16px 8px;
                    border-radius: 0px 0px 8px 8px;

                    .year {
                        color: #aaa;
                        font-size: 14px;
                    }

                    h3 {
                        color: #fff;
                        font-weight: 600;
                        font-size: 18px; // minute 42.52
                    }
                }
            }
        }
    }
</style>