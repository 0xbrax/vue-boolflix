<template>
  <div id="app">
    <TopBar @searchInput="getSearchInput" />
    <MainSection :linkedSearchInput="linkedSearchInput" :linkedMoviesList="moviesList" :linkedSeriesList="seriesList" />
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
      seriesList: []
    }
  },
  methods: {
    getSearchInput(givenSearchInput) {
      this.linkedSearchInput = givenSearchInput;
      if (!this.linkedSearchInput == '') {
        this.getEndpointRequest(this.moviesListRequest);
        this.getSeriesListRequest(); // eliminami, usa funzione unica
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

        this.getMoviesCast(this.moviesList)
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
    getMoviesCast(arrayToCheck) {
      for (let i = 0; i < arrayToCheck.length; i++) {
        if (!this.moviesCastList.includes(arrayToCheck[i].id)) {
          this.moviesCastRequest(arrayToCheck[i].id)
        }
      }

      console.log(this.moviesCastList)
    },
    moviesCastRequest(searchID) {
      axios.get(`https://api.themoviedb.org/3/movie/${searchID}/credits?api_key=a2e57431dfc8a0bdbcd28a2ec61e3e89&query=vacanze&language=it-IT`)
      .then(response => {
          this.moviesCastList.push(response.data.cast.splice(0, 5));
      })
      .catch(error => {
        console.log(error);
      });
    }
  }
}
</script>

<style lang="scss">
@import '~@fortawesome/fontawesome-free/css/all.css';
@import '@/styles/general.scss';
</style>
