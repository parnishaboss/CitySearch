<script setup>
import {ref, watch} from "vue";
import axios from "axios";
import {useRouter} from "vue-router";

const searchQuery = ref('')
const queryTimeout = ref(null)
const searchResults = ref([]);
const searchError = ref(null)
const isLoading = ref(false)

const router = useRouter()


const fetchCities = async (query) => {
  const apiKey = import.meta.env.VITE_OPENWEATHER_API_KEY
  const url = `https://api.openweathermap.org/geo/1.0/direct?q=${query}&limit=5&appid=${apiKey}`

  searchError.value = null
  isLoading.value = true

  try {
    const response = await axios.get(url);
    const cities = response.data;
    if (cities.length > 0) {
      searchResults.value = cities
      searchError.value = null
    } else {
      searchResults.value = []
      searchError.value = "Город не найден. Попробуйте другой запрос."
    }
  } catch (e) {
    searchResults.value = []
    searchError.value = null
    console.error("Error fetching cities:", e);
  } finally {
    isLoading.value = false
  }
}
const getSearchResult = () => {
  clearTimeout(queryTimeout.value)

  queryTimeout.value = setTimeout(() => {
    if (searchQuery.value !== '') {
      fetchCities(searchQuery.value)
    } else {
      searchQuery.value = []
      searchError.value = null
    }
  }, 300)
}

const previewCity = (city) => {
  console.log(city)
  const {name, state} = city
  console.log(name,',', state)
  router.push({
    name: 'cityView',
    params: {city: name, state: state},
    query: {
      lat: city.lat,
      lon: city.lon,
      preview: true,
    }
  })
}
watch(searchQuery, getSearchResult);
</script>

<template>
  <main class="container text-white">
    <div class=" pt-4 mb-8 relative">
      <input type="text" placeholder="Поиск города."
             v-model="searchQuery"
             class="py-2 px-1 w-full bg-transparent border-b focus:border-weather-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E81]">
    </div>
    <div v-if="isLoading" class="flex justify-center items-center">
      <div class="spinner"></div>
    </div>
    <p v-if="searchError && searchResults.length === 0" class="bg-red-500 text-white justify-center flex p-4">
      {{ searchError }}
    </p>
    <ul v-if="searchResults.length > 0 && !isLoading" class="bg-weather-secondary rounded-md p-4">
      <li v-for="(city, index) in searchResults"
          :key="city.id || index"
          class="py-2 px-4 border-b last:border-none"
          @click="previewCity(city)"
      >
        {{ city.name }}, {{ city.country }}
      </li>
    </ul>
  </main>
</template>


<style>
.spinner {
  border: 4px solid rgba(255, 255, 255, 0.2); /* Полупрозрачный бордер */
  border-top: 4px solid #ffffff; /* Верхняя часть спиннера */
  border-radius: 50%; /* Круг */
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite; /* Анимация вращения */
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>