<template>
    <main>
        <section class="container" v-if="linkedMoviesList.length !== 0">
            <h2>Film</h2>
            <div class="flex wrap just-sp-bw">
                <div class="card" v-for="(movie, movieIndex) in linkedMoviesList" :key="movieIndex">

                    <SingleCard :linkedCard="movie" />

                </div>
            </div>
        </section>

        <section class="container" v-if="linkedSeriesList.length !== 0">
            <h2>Serie TV</h2>
            <div class="flex wrap just-sp-bw">
                <div class="card" v-for="(serie, serieIndex) in linkedSeriesList" :key="serieIndex">

                    <SingleCard :linkedCard="serie" />

                </div>
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
        linkedMoviesList: Array,
        linkedSeriesList: Array
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
        aspect-ratio: 1 / 1.2;
        padding: 15px;
    }
</style>