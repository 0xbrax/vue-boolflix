<template>
  <div id="app">
    <TopBar @searchInput="getSearchInput" />
    <MainSection :linkedSearchInput="linkedSearchInput" :linkedMoviesList="moviesList" :linkedMoviesCastList="moviesCastList" :linkedSeriesList="seriesList" :linkedSeriesCastList="seriesCastList" />
  </div>
</template>

<script>
import TopBar from '@/components/TopBar.vue'
import MainSection from '@/components/MainSection.vue'

import axios from 'axios'

export default {
  name: 'App',
  components: {
    TopBar,
    MainSection
  },
  data() {
    return {
      endpoint: 'https://api.themoviedb.org/3/',
      privateAPIkey: 'a2e57431dfc8a0bdbcd28a2ec61e3e89',
      moviesListRequest: 'search/movie',
      moviesCastListRequest: 'movie/',
      seriesListRequest: 'search/tv',
      seriesCastListRequest: 'tv/',
      languageRequest: 'it-IT',
      linkedSearchInput: '',
      moviesList: [],
      moviesCastList: [],
      seriesList: [],
      seriesCastList: []
    }
  },
  methods: {
    getSearchInput(givenSearchInput) {
      this.linkedSearchInput = givenSearchInput;
      if (!this.linkedSearchInput == '') {
        this.getEndpointRequest(this.moviesListRequest, 'moviesList', this.moviesCastListRequest, 'moviesCastList');
        this.getEndpointRequest(this.seriesListRequest, 'seriesList', this.seriesCastListRequest, 'seriesCastList');
      } else {
        this.moviesList = [];
        this.seriesList = [];
      }
    },
    getEndpointRequest(listRequest, arrayOut, castListRequest, castArrayOut) {
      axios.get(`${this.endpoint}${listRequest}?api_key=${this.privateAPIkey}&query=${this.linkedSearchInput}&language=${this.languageRequest}`)
      .then(response => {
        //console.log(response);
        this[arrayOut] = response.data.results;
        this.checkImagePath(this[arrayOut]);
        this.getCastRequest(this[arrayOut], castListRequest, castArrayOut);
      })
      .catch(error => {
        console.log(error);
      });
    },
    checkImagePath(arrayToCheck) {
      for (let i = 0; i < arrayToCheck.length; i++) {
        if (arrayToCheck[i].poster_path == null) {
          arrayToCheck.splice(i, 1)
        }
      }
    },
    getCastRequest(arrayIn, urlRequest, arrayOut) {
      for (let i = 0; i < arrayIn.length; i++) {
        axios.get(`${this.endpoint}${urlRequest}${arrayIn[i].id}/credits?api_key=${this.privateAPIkey}&${this.languageRequest}`)
        .then(response => {
            this[arrayOut].push(response.data.cast.splice(0, 5));
        })
        .catch(error => {
          console.log(error);
        });
      }
    }
  }
}
</script>

<style lang="scss">
@import '~@fortawesome/fontawesome-free/css/all.css';
@import '@/styles/general.scss';
</style>
