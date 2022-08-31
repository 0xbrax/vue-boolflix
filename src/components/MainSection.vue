<template>
    <main>
        <section v-if="linkedMoviesList.length !== 0">
            <h2>Film</h2>
            <div class="flex wrap just-ctr">
                <div v-for="(movie, movieIndex) in linkedMoviesList" :key="movieIndex">

                    <SingleCard :linkedCard="movie" />

                </div>
            </div>
        </section>

        <section v-if="linkedSeriesList.length !== 0">
            <h2>Serie TV</h2>
            <div class="flex wrap just-ctr">
                <div v-for="(serie, serieIndex) in linkedSeriesList" :key="serieIndex">

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

<style lang="scss">

</style>