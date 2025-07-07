<template>
  <div class="glass-modal glass-strong">
    <div class="p-4 bg-transparent rounded-2xl">
      <h2 class="text-2xl font-bold mb-4 text-matte-dark">🔍 Buscar Pokémon</h2>
      <input v-model="pokemonNombre" placeholder="Escribe un nombre de Pokémon..."
             class="w-full border border-gray-600 rounded p-2 mb-2 text-matte-dark bg-transparent" />
      <button @click="fetchPokemon"
              class="w-full bg-green-600 text-white py-2 rounded hover:bg-green-700">
        Buscar
      </button>

      <div v-if="selectedPokemon" class="mt-4 text-center">
        <h3 class="text-xl font-semibold text-matte-dark">{{ selectedPokemon.name }}</h3>
        <img :src="selectedPokemon.sprite" alt="Pokemon Sprite" class="mx-auto mt-2" />
        <p class="text-matte-dark">Generación: {{ selectedPokemon.generation }}</p>
      </div>

      <p v-if="error" class="text-red-600 mt-2">Error: {{ error.message }}</p>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const pokemonNombre = ref('');
const selectedPokemon = ref(null);
const error = ref(null);

const fetchPokemon = async () => {
  const nombre = pokemonNombre.value.trim().toLowerCase();
  error.value = null;
  selectedPokemon.value = null;

  try {
    const res = await fetch(`https://pokeapi.co/api/v2/pokemon/${nombre}`);
    if (!res.ok) throw new Error('No se encontró el Pokémon');
    const data = await res.json();

    const speciesRes = await fetch(data.species.url);
    const speciesData = await speciesRes.json();

    selectedPokemon.value = {
      name: data.name,
      sprite: data.sprites.other.showdown.front_shiny,
      generation: speciesData.generation.name,
    };
  } catch (err) {
    error.value = err;
  }
};
</script>


