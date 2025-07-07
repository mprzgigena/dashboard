<template>
  <div class="relative min-h-screen bg-cover bg-center" style="background-image: url('/imagenfondo2.png');">
    <!-- Puedes quitar estos spans si no los usas -->
    <div class="background">
      <span></span>
      <span></span>
      <span></span>
      <span></span>
      <span></span>
      <span></span>
      <span></span>
      <span></span>
      <span></span>
      <span></span>
    </div>
    <main class="grid grid-cols-1 md:grid-cols-4 gap-4 p-4 min-h-screen relative">
      <!-- Modal de ingreso usuario -->
      <div
        v-if="showModal"
        class="fixed inset-0 z-50 flex items-center justify-center bg-black/40 backdrop-blur-[18px] md:backdrop-blur-[32px]"
      >
        <div
          class="w-full max-w-sm p-8 flex flex-col items-center gap-4
           bg-white/10 border border-white/20 rounded-2xl backdrop-blur-md shadow-2xl"
        >
          <h2 class="text-2xl font-bold mb-2 text-white">
            Bienvenido
          </h2>
          <input
            v-model="usernameInput"
            @keyup.enter="continueToDashboard"
            type="text"
            placeholder="Ingresa tu nombre..."
            class="w-full px-4 py-2 rounded-lg border border-white/30 bg-white/20 text-lg text-white placeholder-white placeholder-opacity-70 focus:outline-none focus:ring-2 focus:ring-blue-400"
          />
          <button
            @click="continueToDashboard"
            class="w-full mt-2 px-6 py-2 rounded-lg bg-blue-600/80 text-white font-semibold shadow hover:bg-blue-700/90 transition"
          >
            Continuar
          </button>
        </div>
      </div>

      <!-- Modal de bienvenida -->
      <div
        v-if="!showModal && username"
        class="md:col-span-4 flex justify-center mb-6"
      >
        <div
          class="px-10 py-6 flex flex-col items-center w-full max-w-2xl
           bg-white/10 border border-white/20 rounded-2xl backdrop-blur-md shadow-lg"
        >
          <div
            class="text-3xl font-extrabold text-black mb-1 text-center"
          >
            Bienvenido {{ username }}, hoy es {{ currentDate }}
          </div>
        </div>
      </div>

      <!-- APIs de clima, divisas y Pokémon -->
      <section
        v-if="!showModal"
        class="md:col-span-2 flex flex-col gap-4"
      >
        <div class="flex flex-col gap-4">
          <div
            class="p-8 flex flex-col items-center gap-4
             bg-white/20 border border-white/20 rounded-2xl backdrop-blur-md shadow-lg"
          >
            <WeatherDashboard />
          </div>
          <div class="flex flex-col md:flex-row gap-4">
            <div
              class="p-8 flex flex-col items-center gap-4
             bg-white/20 border border-white/20 rounded-2xl backdrop-blur-md shadow-lg"
            >
              <CurrencyConverter />
            </div>
            <div
              class="p-8 flex flex-col items-center gap-4
             bg-white/20 border border-white/20 rounded-2xl backdrop-blur-md shadow-lg"
            >
              <PokemonSearch />
            </div>
          </div>
        </div>
      </section>

      <!-- API de Películas -->
      <section
        v-if="!showModal"
        class="md:col-span-2 flex"
      >
        <div
          class="p-8 flex flex-col items-center gap-4
             bg-white/20 border border-white/20 rounded-2xl backdrop-blur-md shadow-lg"
        >
          <MovieRecommender />
        </div>
      </section>
    </main>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import WeatherDashboard from '@/components/WeatherDashboard.vue';
import MovieRecommender from '@/components/MovieRecommender.vue';
import CurrencyConverter from '@/components/CurrencyConverter.vue';
import PokemonSearch from '@/components/PokemonSearch.vue';
import { injectDynamicBackground } from '@/utils/dynamic-bg.js';

const showModal = ref(true);
const usernameInput = ref('');
const username = ref('');

// Fecha de hoy
const currentDate = new Date().toLocaleDateString('es-AR', {
  weekday: 'long', year: 'numeric', month: 'long', day: 'numeric'
});



// Función del modal para ingreso y botón continuar
function continueToDashboard() {
  if (usernameInput.value && usernameInput.value.trim().length > 0) {
    username.value = usernameInput.value.trim();
    // Forzar reactividad y render
    setTimeout(() => {
      showModal.value = false;
      window.scrollTo(0, 0);
    }, 0);
  }
}


</script>

