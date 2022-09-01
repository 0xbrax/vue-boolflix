<template>
    <main>
        <section class="container" v-if="linkedMoviesList.length !== 0">
            <h2>Film</h2>
            <div class="flex wrap">
                <div class="card" v-for="(movie, movieIndex) in linkedMoviesList" :key="movieIndex">

                    <SingleMovieCard :linkedCard="movie" :linkedMoviesCastList="linkedMoviesCastList[movieIndex]" />

                </div>
            </div>
        </section>

        <section class="container" v-if="linkedSeriesList.length !== 0">
            <h2>Serie TV</h2>
            <div class="flex wrap">
                <div class="card" v-for="(serie, serieIndex) in linkedSeriesList" :key="serieIndex">

                    <SingleSerieCard :linkedCard="serie" :linkedSeriesCastList="linkedSeriesCastList[serieIndex]" />

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
        linkedMoviesList: Array,
        linkedMoviesCastList: Array,
        linkedSeriesList: Array,
        linkedSeriesCastList: Array
    }
}
</script>

<style scoped lang="scss">
    main {
        height: calc(100vh - 80px);
        overflow-y: auto;
        background-color: #434343;
        padding: 30px 0;
    }

    h2 {
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
</style>