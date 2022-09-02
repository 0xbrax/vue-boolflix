<template>
    <main>
        <section class="container" v-if="linkedMoviesList.length !== 0">
            <h2>Film</h2>
            <div class="flex wrap mt-15">
                <div class="card" v-for="(movie, movieIndex) in linkedMoviesList" :key="movieIndex">

                    <SingleCard :linkedCard="movie" :linkedCastList="linkedMoviesCastList[movieIndex]" :linkedGenreList="linkedMoviesGenreList[movieIndex]" />

                </div>
            </div>

            <div class="page-container flex just-ctr mt-15">
                <div class="page-wrapper flex">
                    <span class="page" :class="page == moviesPageActive?'active':''" v-for="page in linkedMoviesPage" :key="page" @click="$emit('moviesPageSelected', page)">{{page}}</span>
                </div>
            </div>
        </section>

        <section class="container mt-30" v-if="linkedSeriesList.length !== 0">
            <h2>Serie TV</h2>
            <div class="flex wrap mt-15">
                <div class="card" v-for="(serie, serieIndex) in linkedSeriesList" :key="serieIndex">

                    <SingleCard :linkedCard="serie" :linkedCastList="linkedSeriesCastList[serieIndex]" :linkedGenreList="linkedSeriesGenreList[serieIndex]" />

                </div>
            </div>

            <div class="page-container flex just-ctr mt-15">
                <div class="page-wrapper flex">
                    <span class="page" :class="page == seriesPageActive?'active':''" v-for="page in linkedSeriesPage" :key="page" @click="$emit('seriesPageSelected', page)">{{page}}</span>
                </div>
            </div>
        </section>

        <section class="empty-search" v-if="linkedSearchInput == ''">
            <div>
                Inizia a cercare qualcosa
            </div>
        </section>

        <section class="empty-search" v-if="linkedSearchInput !== '' && linkedMoviesList.length == 0 && linkedSeriesList.length == 0">
            <div>
                Nessun risultato trovato
            </div>
        </section>
    </main>
</template>

<script>
import SingleCard from '@/components/SingleCard.vue'


export default {
    name: 'MainSection',
    components: {
        SingleCard
    },
    props: {
        linkedSearchInput: String,
        linkedMoviesPage: Array,
        moviesPageActive: Number,
        linkedMoviesList: Array,
        linkedMoviesCastList: Array,
        linkedMoviesGenreList: Array,
        linkedSeriesPage: Array,
        seriesPageActive: Number,
        linkedSeriesList: Array,
        linkedSeriesCastList: Array,
        linkedSeriesGenreList: Array
    }
}
</script>

<style scoped lang="scss">
    main {
        height: calc(100vh - 80px);
        overflow-y: auto;
        background-color: #434343;
        padding: 30px 0;
        color: #ffffff;
    }

    h2 {
        font-size: 30px;
    }

    .card {
        width: calc(100% / 5);
        aspect-ratio: 1 / 1.5;
        padding: 15px;
        font-size: 20px;

        &:hover {
            background-color: rgba(255, 255, 255, 0.2);
        }
    }

    .empty-search {
        position: fixed;
        width: 100%;
        height: 100%;
        color: #ffffff;
        text-align: center;
    }

    .page-container {
        width: 100%;

        .page-wrapper {
            max-width: 200px;
            overflow-x: auto;
        }

        .page {
            display: inline-block;
            text-align: center;
            padding: 10px 15px;
            background-color: #000000;
            border: 2px solid #ffffff;

            &:hover {
                cursor: pointer;
                background-color: rgba(255, 255, 255, 0.2);
            }
        }

        .active {
                background-color: rgba(255, 255, 255, 0.2);
            }
    }
</style>