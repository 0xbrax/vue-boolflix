<template>
    <div class="card-container">
        <div class="card-inner">
            <div class="card-front">
                <img class="img-main" :src="imagePath + linkedCard.poster_path" :alt="linkedCard.title">
            </div>

            <div class="card-back flex col align-ctr">
                <h4>{{linkedCard.name}}</h4>
                <div class="card-subtit mt-5"><span class="txt-bold">Titolo originale:</span> {{linkedCard.original_name}}</div>
                <img class="lang-flag mt-5" :src="langFlagEndpoint + checkLangFlag(linkedCard.original_language)" :alt="linkedCard.original_language">

                <div class="mt-5">
                    <i v-for="n in 5" :key="n" class="fa-star" :class="n > getRank(linkedCard.vote_average) ? 'fa-regular' : 'fa-solid'"></i>
                </div>

                <div class="card-cast mt-5">
                    <span class="txt-bold">Cast:</span>
                    <span v-for="(seriesCast, seriesCastListIndex) in linkedSeriesCastList" :key="seriesCastListIndex">
                        <span v-if="seriesCastListIndex < (linkedSeriesCastList.length - 1)"> {{seriesCast.name}},</span>
                        <span v-else> {{seriesCast.name}}</span>
                    </span>
                </div>

                <div class="card-genre mt-5">
                    <span class="txt-bold">Genere:</span>
                    <span v-for="(seriesGenre, seriesGenreListIndex) in linkedSeriesGenreList" :key="seriesGenre.id">
                        <span v-if="seriesGenreListIndex < (linkedSeriesGenreList.length - 1)"> {{seriesGenre.name}},</span>
                        <span v-else> {{seriesGenre.name}}</span>
                    </span>
                </div>

                <div class="card-overview mt-5"><span class="txt-bold">Trama:</span> {{linkedCard.overview}}</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SingleSerieCard',
    props: {
        linkedCard: Object,
        linkedSeriesCastList: Array,
        linkedSeriesGenreList: Array
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