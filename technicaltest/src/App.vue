<template>
  <body >
    
  
  <header class="bg-primary text-white py-4 shadow-sm">
  <div class="container text-center">
    <h1 class="display-5">🎬 Consultar Películas <span class="fs-4 text-warning">OMDb API</span></h1>
  </div>
</header>

  <div>
    
    <MovieFilter v-model="filter" @search="onSearch" />

    
    <div style="margin: 10px 0;">
      <button
      class="btn btn-primary"
        :class="{ active: viewMode === 'table' }"
        @click="viewMode = 'table'"
      >
        📋 Vista Tabla
      </button>
      <button
       class="btn btn-primary"
        :class="{ active: viewMode === 'cards' }"
        @click="viewMode = 'cards'"
      >
        🃏 Vista Tarjetas
      </button>
    </div>

    
    <div v-if="loading">Cargando...</div>
    <div v-if="error" class="error">{{ error }}</div>

    
    <MovieTable v-if="viewMode === 'table'" :movies="movies" />

   
    <div v-else-if="viewMode === 'cards'" class="movie-cards-container">
      <MovieCard
        v-for="movie in movies"
        :key="movie.imdbID"
        :movie="movie"
      />
    </div>

    
    <div v-if="totalResults > 10" class="pagination" style="margin-top: 1em;">
      <button class="btn btn-primary" @click="prevPage" :disabled="currentPage === 1">Anterior</button>
      <span>Página {{ currentPage }} de {{ Math.ceil(totalResults / 10) }}</span>
      <button class="btn btn-primary" @click="nextPage" :disabled="currentPage * 10 >= totalResults">Siguiente</button>
    </div>
  </div>

  <footer class="bg-dark text-light py-3 mt-5 shadow-lg border-top">
  <div class="container text-center">
    <small>
      Prueba FrontEnd - Tecnoglass &copy; 2025 &mdash; Powered by OMDb API
      <br>
      Yesid borrero - yesidborrero@hotmail.com
    </small>

  </div>
</footer>

</body>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import MovieTable from './components/MovieTable.vue'
import MovieFilter from './components/MovieFilter.vue'
import MovieCard from './components/MovieCard.vue' 

const API_KEY = 'ac2fdf88'

const movies = ref([])
const filter = ref('')
const loading = ref(false)
const error = ref('')
const currentPage = ref(1)
const totalResults = ref(0)


const viewMode = ref('table') 

const defaultSearchTerm = 'game of thrones'


const fetchMovies = async (title, page = 1) => {
  const searchTerm = title.trim() || defaultSearchTerm
  if (!searchTerm) {
    movies.value = []
    totalResults.value = 0
    return
  }
  loading.value = true
  error.value = ''
  try {
    const response = await axios.get(
      `https://www.omdbapi.com/?apikey=${API_KEY}&s=${encodeURIComponent(searchTerm)}&page=${page}`
    )
    if (response.data.Search) {
      movies.value = response.data.Search
      totalResults.value = parseInt(response.data.totalResults) || 0
      currentPage.value = page
    } else {
      movies.value = []
      totalResults.value = 0
      error.value = response.data.Error || 'Sin resultados.'
    }
  } catch (e) {
    movies.value = []
    totalResults.value = 0
    error.value = 'Error al consultar la API'
  } finally {
    loading.value = false
  }
}

const onSearch = () => {
  fetchMovies(filter.value, 1)
}

const nextPage = () => {
  if (currentPage.value * 10 < totalResults.value) {
    fetchMovies(filter.value, currentPage.value + 1)
  }
}

const prevPage = () => {
  if (currentPage.value > 1) {
    fetchMovies(filter.value, currentPage.value - 1)
  }
}

fetchMovies('', 1)
</script>

<style scoped>
.error {
  color: red;
  margin: 10px 0;
}

button{
  margin-right: 12px;
}

header{
  margin-bottom: 12px;
}
body {
  background: linear-gradient(120deg, #232526 60%, #161e32 100%);
  color: #f0f0f0;
}

</style>
