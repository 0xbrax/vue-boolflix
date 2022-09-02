<template>
    <div class="card-container">
        <div class="card-inner">
            <div class="card-front">
                <img v-if="linkedCard.poster_path !== null" class="img-main" :src="imagePath + linkedCard.poster_path" :alt="linkedCard.title?linkedCard.title:linkedCard.name">
                <div v-else class="img-replace flex wrap just-ctr align-ctr">
                    <h4>{{linkedCard.title?linkedCard.title:linkedCard.name}}</h4>
                    <img :src="require('@/assets/img/popcorn-icon.png')" :alt="linkedCard.title?linkedCard.title:linkedCard.name">
                </div>
            </div>

            <div class="card-back flex col align-ctr">
                <h5>{{linkedCard.title?linkedCard.title:linkedCard.name}}</h5>
                <div v-if="linkedCard.original_title !== linkedCard.title" class="card-subtit mt-5"><span class="txt-bold">Titolo originale:</span> {{linkedCard.original_title?linkedCard.original_title:linkedCard.original_name}}</div>
                <img class="lang-flag mt-5" :src="langFlagEndpoint + checkLangFlag(linkedCard.original_language)" :alt="linkedCard.original_language">

                <div v-if="linkedCard.vote_average !== 0" class="mt-5">
                    <i v-for="n in 5" :key="n" class="fa-star" :class="n > getRank(linkedCard.vote_average) ? 'fa-regular' : 'fa-solid'"></i>
                </div>

                <div v-show="linkedCastList.length !== 0" class="card-cast mt-5">
                    <span class="txt-bold">Cast:</span>
                    <span v-for="(cast, castListIndex) in linkedCastList" :key="castListIndex">
                        <span v-if="castListIndex < (linkedCastList.length - 1)"> {{cast.name}},</span>
                        <span v-else> {{cast.name}}</span>
                    </span>
                </div>

                <div v-show="linkedGenreList.length !== 0" class="card-genre mt-5">
                    <span class="txt-bold">Genere:</span>
                    <span v-for="(genre, genreListIndex) in linkedGenreList" :key="genre.id">
                        <span v-if="genreListIndex < (linkedGenreList.length - 1)"> {{genre.name}},</span>
                        <span v-else> {{genre.name}}</span>
                    </span>
                </div>

                <div v-if="linkedCard.overview !== ''" class="card-overview mt-5"><span class="txt-bold">Trama:</span> {{linkedCard.overview}}</div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SingleCard',
    props: {
        linkedCard: Object,
        linkedCastList: Array,
        linkedGenreList: Array
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
    .card-container {
        height: 100%;
        width: 100%;
        background-color: transparent;
        perspective: 1000px;
        color: #ffffff;
        text-align: center;
    }

    .card-inner {
        height: 100%;
        width: 100%;
        outline: 3px solid rgba(255, 255, 255, 0.4);
        outline-offset: 3px;
        position: relative;
        transition: transform 0.8s 0.2s;
        transform-style: preserve-3d;
    }

    .card-front, .card-back {
        position: absolute;
        width: 100%;
        height: 100%;
        -webkit-backface-visibility: hidden; /* Safari */
        backface-visibility: hidden;
    }

    .card-front {
        overflow: hidden;
        background-color: #0a0a0a;

        h4 {
            width: 100%;
        }
    }

    .card-back {
        transform: rotateY(180deg);
        background-color: #2b2b2b;
        padding: 10px;
    }

    .card-container:hover .card-inner {
        transform: rotateY(180deg);
    }

    .img-main {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .img-replace {
        height: 100%;
        padding: 10px;
    }

    .card-subtit, 
    .card-cast, 
    .card-genre {
        font-size: 12px;
    }

    .lang-flag {
        height: 20px;
    }

    .card-overview {
        overflow-y: auto;
        font-size: 14px;
    }
</style>