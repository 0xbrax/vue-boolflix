<template>
    <div class="card-container">
        <div class="card-inner">
            <div class="card-front">
                <img class="img-main" :src="imagePath + linkedCard.poster_path" :alt="linkedCard.title">
            </div>

            <div class="card-back flex col align-ctr">
                <h3>{{linkedCard.title}}</h3>
                <div>{{linkedCard.original_title}}</div>
                <img class="lang-flag" :src="langFlagEndpoint + checkLangFlag(linkedCard.original_language)" :alt="linkedCard.original_language">

                <div>
                    <i v-for="n in 5" :key="n" class="fa-star" :class="n > getRank(linkedCard.vote_average) ? 'fa-regular' : 'fa-solid'"></i>
                </div>

                <div v-for="(moviesCast, moviesCastListIndex) in linkedMoviesCastList" :key="moviesCastListIndex">
                    {{moviesCast.name}}
                </div>

                <div v-for="(moviesGenre) in linkedMoviesGenreList" :key="moviesGenre.id">
                    {{moviesGenre.name}}
                </div>

                <div class="card-overview">{{linkedCard.overview}}</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SingleMovieCard',
    props: {
        linkedCard: Object,
        linkedMoviesCastList: Array,
        linkedMoviesGenreList: Array
    },
    data() {
        return {
            imagePath: 'https://image.tmdb.org/t/p/w342',
            langFlagEndpoint: 'https://countryflagsapi.com/svg/'
        }
    }, 
    methods: {
        checkLangFlag(lang) {
            let checkLang = lang.substring(0, 2);
            if (checkLang == 'en') {
                return checkLang = 'gb';
            } else if (checkLang == 'zh') {
                return checkLang = 'cn';
            } else if (checkLang == 'ja') {
                return checkLang = 'jp';
            } else if (checkLang == 'ko') {
                return checkLang = 'kr';
            } else if (checkLang == 'da') {
                return checkLang = 'dk';
            } else if (checkLang == 'el') {
                return checkLang = 'gr';
            } else {
                return checkLang
            }
        },
        getRank(vote) {
            return Math.ceil(vote / 2);
        }
    }
}
</script>

<style scoped lang="scss">

</style>