<template>
    <main>
        <section v-if="linkedMoviesList.length !== 0">
            <h2>Film</h2>
            <div class="flex wrap just-ctr">
                <div v-for="(movie, movieIndex) in linkedMoviesList" :key="movieIndex">
                    <img :src="getImagePath(movie.backdrop_path)" alt="">
                    <h3>{{movie.title}}</h3>
                    <div>{{movie.original_title}}</div>
                    <div>{{movie.original_language}}</div>
                    <div>{{getLanguageFlag(movie.original_language)}}</div>
                    <div>{{movie.vote_average}}</div>
                </div>
            </div>
        </section>

        <section v-if="linkedSeriesList.length !== 0">
            <h2>Serie TV</h2>
            <div class="flex wrap just-ctr">
                <div v-for="(serie, serieIndex) in linkedSeriesList" :key="serieIndex">
                    <img :src="getImagePath(serie.backdrop_path)" alt="">
                    <h3>{{serie.name}}</h3>
                    <div>{{serie.original_name}}</div>
                    <div>{{serie.original_language}}</div>
                    <div>{{serie.vote_average}}</div>
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
            languageList: [
                {
                    lang: 'us',
                    emojiP1: '127482',
                    emojiP2: '127480'
                },
                {
                    lang: 'es',
                    emojiP1: '127466',
                    emojiP2: '127480'
                },
                {
                    lang: 'it',
                    emojiP1: '127470',
                    emojiP2: '127481'
                }
            ]
        }
    },
    methods: {
        getLanguageFlag(language) {
            console.log((String.fromCodePoint(this.languageList[2].emojiP1) + String.fromCodePoint(this.languageList[2].emojiP2)))

            let moviesFlag = 'flag test';
            // !!! TO FIX

            for (let i = 0; i < this.languageList; i++) {
                console.log('hello world')

                if (language == this.languageList[i].lang) {
                    moviesFlag = (String.fromCodePoint(this.languageList[i].emojiP1) + String.fromCodePoint(this.languageList[i].emojiP2))
                }
                return moviesFlag
            }

            return moviesFlag
        },
        getImagePath(path) {
            console.log(path)
            return require('https://image.tmdb.org/t/p/w342' + path)
        }
    }
}
</script>

<style lang="scss">

</style>