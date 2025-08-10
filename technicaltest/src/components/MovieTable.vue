<template>
  <table>
    <thead>
      <tr>
        <th>Poster</th>
        <th>Título</th>
        <th>Año</th>
        <th>Tipo</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="movie in movies" :key="movie.imdbID">
        <td>
          <img :src="movie.Poster" alt="Poster" width="60" v-if="movie.Poster !== 'N/A'" />
        </td>
        <td>{{ movie.Title }}</td>
        <td>{{ movie.Year }}</td>
        <td>{{ movie.Type }}</td>
      </tr>
      <tr v-if="movies.length === 0">
        <td colspan="4">No se encontraron películas</td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
import {defineProps} from 'vue';

defineProps({
  movies: {
    type: Array,
    required: true
  }
});
</script>
