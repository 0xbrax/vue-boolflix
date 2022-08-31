<template>
    <div class="card-container">
        <div class="card-inner">
            <div class="card-front">
                <img class="img-main" :src="imagePath + linkedCard.backdrop_path" :alt="linkedCard.title">
            </div>

            <div class="card-back">
                <h3>{{linkedCard.title}}</h3>
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
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SingleCard',
    props: {
        linkedCard: Object
    },
    data() {
        return {
            imagePath: 'https://image.tmdb.org/t/p/h632',
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
    .lang-flag {
        height: 20px;
    }

    .card-container {
        height: 100%;

        perspective: 1000px;
    }

    .card-inner {
        height: 100%;
        background-color: aqua;
        outline: 3px solid rgba(255, 255, 255, 0.8);
        outline-offset: 3px;

        

        position: relative;
        transition: transform 0.8s;
        transform-style: preserve-3d;

        &:hover {
            transform: rotateY(180deg);
        }
    }

    .card-front {
        height: 100%;
        overflow: hidden;
    }

    .img-main {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }

    .card-back {
        height: 100%;
        overflow-y: auto;
    }

    .card-front, .card-back {
        position: absolute;
        width: 100%;
        height: 100%;
        -webkit-backface-visibility: hidden; /* Safari */
        backface-visibility: hidden;
    }
</style>