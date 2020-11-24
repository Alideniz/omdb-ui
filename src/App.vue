<template>
  <div id="app">
    <div class="ui container center aligned">
      <div class="ui form">
        <div class="four fields">
          <div class="field">
            <label>Title</label>
            <input type="text" v-model="searchKey"/>
          </div>
          <div class="field">
            <label>Year</label>
            <input type="number" min="1900" max="2020" v-model="year"/>
          </div>
          <div class="field">
            <label>Type</label>
            <select class="ui compact selection dropdown" v-model="type">
              <option value="movie">Movie</option>
              <option value="series">Series</option>
              <option value="episode">Episode</option>
            </select>
          </div>
          <div class="field">
            <button class="ui primary button" style="margin-top: 24px" type="button" @click="filter">Filter</button>
          </div>
        </div>
      </div>
    </div>
    <LoadingSpinner v-if="loading"/>
    <MovieListContainer v-else :movies="movies"/>
    <div v-if="isPaginationButtonsVisible" class="ui container pagination">
      <button class="ui button" v-if="isPreviousButtonVisible" @click="previousPage">
        <i class="angle double left icon"/>
        Previous
      </button>
      <button class="ui button" v-if="isNextButtonVisible" @click="nextPage">
        Next
        <i class="angle double right icon"/>
      </button>
    </div>
  </div>
</template>

<script>

import MovieListContainer from "@/components/MovieListContainer";
import LoadingSpinner from "@/components/LoadingSpinner";

export default {
  name: 'App',
  components: {LoadingSpinner, MovieListContainer},
  data: () => ({
    searchKey: '',
    year: 2020,
    type: 'movie',
    movies: [],
    loading: false,
    page: 1,
    totalResults: 0
  }),
  created() {
    window.API_KEY = "ef77f4f7";
  },
  methods: {
    async filter() {
      let URL = `http://www.omdbapi.com/?apikey=${window.API_KEY}`
      if (this.searchKey.length > 0) {
        URL += `&s=${this.searchKey}`;
      }
      if (this.year.length > 0) {
        URL += `&y=${this.year}`;
      }

      if (this.page > 1) {
        URL += `&page=${this.page}`;
      }
      if (this.type.length > 0) {
        URL += `&type=${this.type}`;
      }
      this.loading = true;
      let r = await fetch(URL);
      let response = await r.json();
      this.loading = false;
      if (response.Error) {
        alert(response.Error);
      } else {
        this.movies = response.Search;
        this.totalResults = response.totalResults;
      }
    },
    async nextPage() {
      this.page++;
      await this.filter();
    },
    async previousPage() {
      this.page--;
      await this.filter();
    }
  },
  computed: {
    isPaginationButtonsVisible() {
      return this.totalResults > (10 * this.page);
    },
    isNextButtonVisible() {
      return this.totalResults > (10 * this.page);
    },
    isPreviousButtonVisible() {
      return this.page > 1;
    }
  }
}
</script>
<style scoped>
.pagination {
  display: flex;
  justify-content: space-around;
  margin-top: 20px;
  margin-bottom: 20px;
}

</style>