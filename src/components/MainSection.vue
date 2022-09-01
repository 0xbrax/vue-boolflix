<template>
    <main>
        <section class="container" v-if="linkedMoviesList.length !== 0">
            <h2>Film</h2>
            <div class="flex wrap">
                <div class="card" v-for="(movie, movieIndex) in linkedMoviesList" :key="movieIndex">

                    <SingleMovieCard :linkedCard="movie" :linkedMoviesCastList="linkedMoviesCastList[movieIndex]" :linkedMoviesGenreList="linkedMoviesGenreList[movieIndex]" />

                </div>
            </div>

            <div class="page-container">
                <span class="page" v-for="page in linkedMoviesPage" :key="page" @click="$emit('pageSelected', page)">{{page}}</span>
            </div>
        </section>

        <section class="container" v-if="linkedSeriesList.length !== 0">
            <h2>Serie TV</h2>
            <div class="flex wrap">
                <div class="card" v-for="(serie, serieIndex) in linkedSeriesList" :key="serieIndex">

                    <SingleSerieCard :linkedCard="serie" :linkedSeriesCastList="linkedSeriesCastList[serieIndex]" :linkedSeriesGenreList="linkedSeriesGenreList[serieIndex]" />

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
import SingleMovieCard from '@/components/SingleMovieCard.vue'
import SingleSerieCard from '@/components/SingleSerieCard.vue'


export default {
    name: 'MainSection',
    components: {
        SingleMovieCard,
        SingleSerieCard
    },
    props: {
        linkedSearchInput: String,
        linkedMoviesPage: Array,
        linkedMoviesList: Array,
        linkedMoviesCastList: Array,
        linkedMoviesGenreList: Array,
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

    .card {
        width: calc(100% / 5);
        aspect-ratio: 1 / 1.5;
        padding: 15px;
    }

    .empty-search {
        position: fixed;
        width: 100%;
        height: 100%;
        color: #ffffff;
        text-align: center;
    }

    .page-container {
        text-align: center;

        .page {
            display: inline-block;
            padding: 10px;
            background-color: black;
            border: 1px solid white;
        }
    }
</style>