<template>
    <div class="card-container">
        <div class="card-inner">
            <div class="card-front">
                <img class="img-main" :src="imagePath + linkedCard.poster_path" :alt="linkedCard.title">
            </div>

            <div class="card-back flex col align-ctr">
                <h3>{{linkedCard.title}}</h3>
                <div>{{linkedCard.id}}</div>
                <div>{{linkedCard.original_title}}</div>
                <img class="lang-flag" :src="langFlagEndpoint + checkLangFlag(linkedCard.original_language)" :alt="linkedCard.original_language">

                <div>
                    <div v-if="getRank(linkedCard.vote_average) == 1"><i class="fa-solid fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i></div>
                    <div v-else-if="getRank(linkedCard.vote_average) == 2"><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i></div>
                    <div v-else-if="getRank(linkedCard.vote_average) == 3"><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i></div>
                    <div v-else-if="getRank(linkedCard.vote_average) == 4"><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-regular fa-star"></i></div>
                    <div v-else-if="getRank(linkedCard.vote_average) == 5"><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i><i class="fa-solid fa-star"></i></div>
                    <div v-else><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i><i class="fa-regular fa-star"></i></div>
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
        linkedCard: Object
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