<template>
  <div id="app">
    <TopBar @searchInput="getSearchInput" />
    <MainSection :linkedMoviesList="moviesList" />
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
      linkedSearchInput: '',
      moviesList: []
    }
  },
  methods: {
    getSearchInput(givenSearchInput) {
      this.linkedSearchInput = givenSearchInput;
      if (!this.linkedSearchInput == '') {
        this.moviesListRequest();
      } else {
        this.moviesList = [];
      }
    },
    moviesListRequest() {
      axios.get(`https://api.themoviedb.org/3/search/movie?api_key=a2e57431dfc8a0bdbcd28a2ec61e3e89&query=${this.linkedSearchInput}&language=it-IT`)
      .then(response => {
        //console.log(response);
        this.moviesList = response.data.results;

        console.log(this.moviesList);
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
