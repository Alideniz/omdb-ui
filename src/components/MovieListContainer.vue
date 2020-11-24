<template>
  <div class="ui four column doubling stackable grid container">
    <div class="column" v-for="movie in movies" :key="movie.imdbID">
      <movie :movie="movie"></movie>
    </div>
  </div>
</template>
<script>
import Movie from "@/components/Movie"

export default {
  name: 'MovieListContainer',
  components: {Movie},
  props: {
    movies: {
      type: Array,
      required: true
    }
  }
}
</script>
