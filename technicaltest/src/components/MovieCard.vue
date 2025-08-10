<template>
  <div class="movie-card">
    <img
      :src="movie.Poster !== 'N/A' ? movie.Poster : placeholderImage"
      alt="Poster"
      class="movie-poster"
    />
    <div class="movie-details">
      <h3>{{ movie.Title }}</h3>
      <p><strong>Año:</strong> {{ movie.Year }}</p>
      <p><strong>Tipo:</strong> {{ movie.Type }}</p>
      <p v-if="movie.imdbID"><strong>ID:</strong> {{ movie.imdbID }}</p>
    </div>
  </div>
</template>

<script setup>
import {defineProps} from 'vue'

const placeholderImage = 'https://via.placeholder.com/150x220?text=No+Image'

defineProps({
  movie: {
    type: Object,
    required: true
  }
})
</script>

<style scoped>
.movie-card {
  width: 160px;
  border-radius: 8px;
  box-shadow: 0 2px 8px #0002;
  margin: 10px;
  background: #fff;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  transition: transform 0.18s;
  cursor: pointer;
}
.movie-card:hover {
  transform: scale(1.03);
  box-shadow: 0 4px 16px #0003;
}
.movie-poster {
  width: 100%;
  height: 220px;
  object-fit: cover;
  background: #efefef;
}
.movie-details {
  flex: 1;
  padding: 10px;
}
.movie-details h3 {
  margin: 6px 0 10px 0;
  font-size: 1rem;
  font-weight: bold;
}
.movie-details p {
  margin: 4px 0;
  font-size: 0.9rem;
  color: #444;
}
</style>
