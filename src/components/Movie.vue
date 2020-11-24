<template>
  <div class="ui card">
    <div class="image">
      <img :src="imageURL" @error="onError">
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
    },
  },
  data() {
    return {
      imageURL: ''
    }
  },
  created() {
    this.imageURL = `http://img.omdbapi.com/?apikey=${window.API_KEY}&i=${this.movie.imdbID}`;
  },
  methods: {
    onError() {
      this.imageURL = `data:image/gif;base64,R0lGODlhAQABAAAAACwAAAAAAQABAAA=`;
    }
  }
}
</script>

<style scoped>
</style>