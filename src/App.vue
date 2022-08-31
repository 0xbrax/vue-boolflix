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
      seriesListRequest: 'search/tv',
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
        this.getEndpointRequest(this.moviesListRequest);
        this.getSeriesListRequest(); // test eliminami, usa funzione unica
      } else {
        this.moviesList = [];
        this.seriesList = [];
      }
    },
    getEndpointRequest(listRequest) {
      ///////// USA UNA FUNZIONE SOLA, PROBLEMA SECONDA VARIABILE DENTRO THEN

      axios.get(`${this.endpoint}${listRequest}?api_key=${this.privateAPIkey}&query=${this.linkedSearchInput}&language=${this.languageRequest}`)
      .then(response => {
        //console.log(response);
        this.moviesList = response.data.results;
        this.checkImagePath(this.moviesList);

        this.getMoviesCastRequest(this.moviesList);
      })
      .catch(error => {
        console.log(error);
      });
    },


    getSeriesListRequest() {
      axios.get(`${this.endpoint}${this.seriesListRequest}?api_key=${this.privateAPIkey}&query=${this.linkedSearchInput}&language=${this.languageRequest}`)
      .then(response => {
        //console.log(response);
        this.seriesList = response.data.results;
        this.checkImagePath(this.seriesList);
        this.getSeriesCastRequest(this.seriesList);
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
    getMoviesCastRequest(array) {
      for (let i = 0; i < array.length; i++) {
        axios.get(`${this.endpoint}movie/${array[i].id}/credits?api_key=${this.privateAPIkey}&${this.languageRequest}`)
        .then(response => {
            this.moviesCastList.push(response.data.cast.splice(0, 5));
        })
        .catch(error => {
          console.log(error);
        });
      }
    },
    getSeriesCastRequest(array) {
      for (let i = 0; i < array.length; i++) {
        axios.get(`${this.endpoint}tv/${array[i].id}/credits?api_key=${this.privateAPIkey}&${this.languageRequest}`)
        .then(response => {
            this.seriesCastList.push(response.data.cast.splice(0, 5));
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
