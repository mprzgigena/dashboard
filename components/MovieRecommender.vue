<template>
  <div class="glass-modal glass-strong">
    <div class="p-4 bg-transparent rounded-2xl">
      <h2 class="text-2xl font-bold mb-4 text-matte-dark">🎬 Películas populares</h2>
      <div v-if="movies.length">
        <ul class="grid md:grid-cols-3 gap-4">
          <li v-for="movie in movies" :key="movie.id" class="bg-transparent rounded-xl p-2">
            <img
              :src="'https://image.tmdb.org/t/p/w300' + movie.poster_path"
              :alt="movie.title"
              class="rounded mb-2 w-full"
            />
            <h3 class="font-semibold text-lg text-matte-dark">{{ movie.title }}</h3>
            <p class="text-sm text-matte-dark">{{ movie.overview.slice(0, 100) }}...</p>
          </li>
        </ul>
      </div>
      <div v-else>
        <p class="text-matte-dark">Cargando películas...</p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const API_KEY = 'cc71509e4590a3b774c82ac81ebfc697';
const url = `https://api.themoviedb.org/3/trending/movie/week?api_key=${API_KEY}`;

const { data, pending, error } = await useFetch(url);
const movies = ref([]);

if (data.value && data.value.results) {
  movies.value = data.value.results.slice(0, 6);
}
</script>

