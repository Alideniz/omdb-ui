<template>
  <div class="ui card">
    <div class="image">
      <img :src="imgURL">
    </div>
    <div class="content">
      <a class="header">{{ movie.Title }}</a>
      <div class="meta">
        <span class="date">{{ movie.Year }}</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Movie",
  props: {
    movie: {
      type: Object,
      required: true
    }
  },
  computed: {
    imgURL() {
      return `http://img.omdbapi.com/?apikey=${window.API_KEY}&i=${this.movie.imdbID}`;
    }
  }
}
</script>

<style scoped>
</style>