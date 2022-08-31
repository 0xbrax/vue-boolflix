<template>
    <main>
        <section v-if="linkedMoviesList.length !== 0">
            <h2>Film</h2>
            <div class="flex wrap just-ctr">
                <div v-for="(movie, movieIndex) in linkedMoviesList" :key="movieIndex">
                    <img :src="imagePath + movie.backdrop_path" :alt="movie.title">
                    <h3>{{movie.title}}</h3>
                    <div>{{movie.original_title}}</div>
                    <img class="lang-flag" :src="langFlagEndpoint + movie.original_language" :alt="movie.original_language">
                    <div>{{getRank(movie.vote_average)}}</div>
                    <div><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i></div>
                </div>
            </div>
        </section>

        <section v-if="linkedSeriesList.length !== 0">
            <h2>Serie TV</h2>
            <div class="flex wrap just-ctr">
                <div v-for="(serie, serieIndex) in linkedSeriesList" :key="serieIndex">
                    <img :src="imagePath + serie.backdrop_path" :alt="serie.title">
                    <h3>{{serie.name}}</h3>
                    <div>{{serie.original_name}}</div>
                    <img class="lang-flag" :src="langFlagEndpoint + serie.origin_country" :alt="serie.origin_countrye">
                    <div>{{getRank(serie.vote_average)}}</div>
                </div>
            </div>
        </section>
    </main>
</template>

<script>
export default {
    name: 'MainSection',
    props: {
        linkedMoviesList: Array,
        linkedSeriesList: Array
    },
    data() {
        return {
            imagePath: 'https://image.tmdb.org/t/p/w342',
            langFlagEndpoint: 'https://countryflagsapi.com/svg/'
        }
    }, 
    methods: {
        getRank(vote) {
            console.log('voto originale: ' + vote)
            let ranked = Math.ceil(vote / 2);
            console.log('voto nuovo: ' + ranked)

            if (ranked == 0) {
                return '<i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i>'
            } else if (ranked == 1) {
                return '<i class="fa-solid fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i>'
            } else if (ranked == 2) {
                return '<i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i>'
            } else if (ranked == 3) {
                return '<i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i>'
            } else if (ranked == 4) {
                return '<i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-regular fa-star"></i>'
            } else if (ranked == 5) {
                return '<i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i>'
            }
        }
    }
}
</script>

<style scoped lang="scss">
    .lang-flag {
        height: 20px;
    }
</style>