<template>
    <div>
        <ul>
            <li v-for="(movie, movieIndex) in getMoviesList" :key="movieIndex">
                {{movie.title}}
            </li>
        </ul>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'MainSection',
    props: {
        searchedInput: String
    },
    data() {
        return {
            moviesList: []
        }
    },
    methods: {
        moviesListRequest() {
            axios.get(`https://api.themoviedb.org/3/search/movie?api_key=a2e57431dfc8a0bdbcd28a2ec61e3e89&query=${this.searchedInput}&language=it-IT`)
            .then(response => {
                //console.log(response);
                this.moviesList = response.data.results;

                console.log(this.moviesList)
            })
            .catch(error => {
                console.log(error);
            });
        }
    },
    computed: {
        getMoviesList() {
            const filteredMoviesList = [];

            if (this.searchedInput == '') {
                return filteredMoviesList
            } else {
                this.moviesListRequest();
                return this.moviesList
            }
        }
    }
}
</script>

<style lang="scss">

</style>