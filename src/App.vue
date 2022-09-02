<template>
  <div id="app">
    <TopBar @searchInput="getSearchInput" />

    <MainSection :linkedSearchInput="linkedSearchInput" 
      :linkedMoviesPage="moviesPage" @moviesPageSelected="getMoviesPageSelected" :moviesPageActive="moviesPageActive" 
        :linkedMoviesList="moviesList" :linkedMoviesCastList="moviesCastList" :linkedMoviesGenreList="moviesGenreList" 
      :linkedSeriesPage="seriesPage" @seriesPageSelected="getSeriesPageSelected" :seriesPageActive="seriesPageActive" 
        :linkedSeriesList="seriesList" :linkedSeriesCastList="seriesCastList" :linkedSeriesGenreList="seriesGenreList" 
    />
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
      moviesSpecRequest: 'movie/',
      seriesListRequest: 'search/tv',
      seriesSpecRequest: 'tv/',
      languageRequest: 'it-IT',
      linkedSearchInput: '',
      moviesPage: [],
      moviesPageActive: 1,
      moviesList: [],
      moviesCastList: [],
      moviesGenreList: [],
      seriesPage: [],
      seriesPageActive: 1,
      seriesList: [],
      seriesCastList: [],
      seriesGenreList:[]
    }
  },
  methods: {
    getSearchInput(givenSearchInput) {
      this.linkedSearchInput = givenSearchInput;
      if (!this.linkedSearchInput == '') {
        this.getEndpointRequest(this.moviesListRequest, 'moviesList', this.moviesSpecRequest, 'moviesCastList', 'moviesGenreList', 'moviesPage');
        this.getEndpointRequest(this.seriesListRequest, 'seriesList', this.seriesSpecRequest, 'seriesCastList', 'seriesGenreList', 'seriesPage');
      } else {
        this.moviesList = [];
        this.seriesList = [];
      }
    },
    getEndpointRequest(listRequest, arrayOut, specListRequest, castArrayOut, genreArrayOut, arrayPage) {
      axios.get(`${this.endpoint}${listRequest}?api_key=${this.privateAPIkey}&query=${this.linkedSearchInput}&language=${this.languageRequest}`)
      .then(response => {
        this[arrayOut] = response.data.results;

        this.getSearchByPage(response.data.total_pages, arrayPage);

        this.getCastRequest(this[arrayOut], specListRequest, castArrayOut);
        this.getGenreRequest(this[arrayOut], specListRequest, genreArrayOut);
      })
      .catch(error => {
        console.log(error);
      });
    },
    getSearchByPage(page, arrayPage) {
      this[arrayPage] = [];
      for (let i = 1; i <= page; i++) {
        this[arrayPage].push(i);
      }
    },
    getMoviesPageSelected(givenPage) {
      axios.get(`${this.endpoint}${this.moviesListRequest}?api_key=${this.privateAPIkey}&query=${this.linkedSearchInput}&language=${this.languageRequest}&page=${givenPage}`)
      .then(response => {
        this.moviesList = response.data.results;
        this.getCastRequest(this.moviesList, this.moviesSpecRequest, 'moviesCastList');
        this.getGenreRequest(this.moviesList, this.moviesSpecRequest, 'moviesGenreList');
        this.moviesPageActive = givenPage;
      })
      .catch(error => {
        console.log(error);
      });
    },
    getSeriesPageSelected(givenPage) {
      axios.get(`${this.endpoint}${this.seriesListRequest}?api_key=${this.privateAPIkey}&query=${this.linkedSearchInput}&language=${this.languageRequest}&page=${givenPage}`)
      .then(response => {
        this.seriesList = response.data.results;
        this.getCastRequest(this.seriesList, this.seriesSpecRequest, 'seriesCastList');
        this.getGenreRequest(this.seriesList, this.seriesSpecRequest, 'seriesGenreList');
        this.seriesPageActive = givenPage;
      })
      .catch(error => {
        console.log(error);
      });
    },
    getCastRequest(arrayIn, urlRequest, arrayOut) {
      this[arrayOut] = [];
      for (let i = 0; i < arrayIn.length; i++) {
        axios.get(`${this.endpoint}${urlRequest}${arrayIn[i].id}/credits?api_key=${this.privateAPIkey}&${this.languageRequest}`)
        .then(response => {
          this[arrayOut].push(response.data.cast.splice(0, 5));
        })
        .catch(error => {
          console.log(error);
        });
      }
    },
    getGenreRequest(arrayIn, urlRequest, arrayOut) {
      this[arrayOut] = [];
      for (let i = 0; i < arrayIn.length; i++) {
        axios.get(`${this.endpoint}${urlRequest}${arrayIn[i].id}?api_key=${this.privateAPIkey}&${this.languageRequest}`)
        .then(response => {
          this[arrayOut].push(response.data.genres.splice(0, 2));
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
