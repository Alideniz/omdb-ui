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
            <button class="ui button" style="margin-top: 24px" type="button" @click="filter">Filter</button>
          </div>

        </div>
      </div>

    </div>
    <div v-if="loading" class="ui segment">
      <div class="ui active inverted dimmer">
        <div class="ui text loader">Loading</div>
      </div>
      <p></p>
    </div>
    <div v-else class="ui four column doubling stackable grid container">
      <div class="column" v-for="movie in movies" :key="movie.imdbID">
        <movie :movie="movie"></movie>
      </div>
    </div>
  </div>
</template>

<script>

import Movie from "@/components/Movie";

export default {
  name: 'App',
  components: {Movie},
  data: () => ({
    searchKey: '',
    year: 2020,
    type: 'movie',
    movies: [],
    loading: false
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
      }
    }
  }
}
</script>