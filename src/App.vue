<template>
  <div id="app">
    <TopBar @searchInput="getSearchInput" />
    <MainSection :linkedSearchInput="linkedSearchInput" :linkedMoviesPage="moviesPage" @pageSelected="getPageSelected" :linkedMoviesList="moviesList" :linkedMoviesCastList="moviesCastList" :linkedMoviesGenreList="moviesGenreList" :linkedSeriesList="seriesList" :linkedSeriesCastList="seriesCastList" :linkedSeriesGenreList="seriesGenreList" />
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
      moviesPage: [],
      moviesList: [],
      moviesCastList: [],
      moviesGenreList: [],
      seriesList: [],
      seriesCastList: [],
      seriesGenreList:[]
    }
  },
  methods: {
    getSearchInput(givenSearchInput) {
      this.linkedSearchInput = givenSearchInput;
      if (!this.linkedSearchInput == '') {
        this.getEndpointRequest(this.moviesListRequest, 'moviesList', this.moviesCastListRequest, 'moviesCastList', 'moviesGenreList', 'moviesPage');
        //this.getEndpointRequest(this.seriesListRequest, 'seriesList', this.seriesCastListRequest, 'seriesCastList', 'seriesGenreList');
      } else {
        this.moviesList = [];
        this.seriesList = [];
      }
    },
    getPageSelected(givenPage) {
      console.log(givenPage)

      axios.get(`${this.endpoint}${this.moviesListRequest}?api_key=${this.privateAPIkey}&query=${this.linkedSearchInput}&language=${this.languageRequest}&page=${givenPage}`)
      .then(response => {

        this.moviesList = response.data.results;

        //this.checkImagePath(this[arrayOut]);
        //this.getCastRequest(this[arrayOut], castListRequest, castArrayOut);
        //this.getGenreRequest(this[arrayOut], castListRequest, genreArrayOut);
      })
      .catch(error => {
        console.log(error);
      });
    },
    getEndpointRequest(listRequest, arrayOut, castListRequest, castArrayOut, genreArrayOut, arrayPage) {
      axios.get(`${this.endpoint}${listRequest}?api_key=${this.privateAPIkey}&query=${this.linkedSearchInput}&language=${this.languageRequest}`)
      .then(response => {

        this[arrayOut] = response.data.results;

        this.getSearchByPage(response.data.total_pages, arrayPage);

        this.checkImagePath(this[arrayOut]);
        this.getCastRequest(this[arrayOut], castListRequest, castArrayOut);
        this.getGenreRequest(this[arrayOut], castListRequest, genreArrayOut);
      })
      .catch(error => {
        console.log(error);
      });
    },

    getSearchByPage(page, arrayPage) {
      /////TEST

      console.log(page)

      this[arrayPage] = [];
      for (let i = 1; i <= page; i++) {
        this[arrayPage].push(i);
      }

      console.log(this[arrayPage])
    },

    checkImagePath(arrayToCheck) {
      for (let i = 0; i < arrayToCheck.length; i++) {
        if (arrayToCheck[i].poster_path == null) {
          arrayToCheck.splice(i, 1)
        }
      }
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
