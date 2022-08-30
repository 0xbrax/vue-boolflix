<template>
    <main>
        <section v-if="linkedMoviesList.length !== 0">
            <div v-for="(movie, movieIndex) in linkedMoviesList" :key="movieIndex">
                <h3>{{movie.title}}</h3>
                <div>{{movie.original_title}}</div>
                <div>{{movie.original_language}}</div>
                <div>{{movie.vote_average}}</div>
            </div>
        </section>
    </main>
</template>

<script>
export default {
    name: 'MainSection',
    props: {
        linkedMoviesList: Array
    }
}
</script>

<style lang="scss">

</style>