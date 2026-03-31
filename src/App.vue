<script setup>
import { computed, onMounted, ref } from "vue";
import CountryCard from "./components/CountryCard.vue";
import SearchBar from "./components/SearchBar.vue";

const countries = ref([]);
const search = ref("");
const loading = ref(false);
const error = ref(null);

const title = ref("Country Explorer");
const subTitle = ref("Discover countries, capitals and populations");

const fetchCountries = async () => {
  loading.value = true;
  error.value = null;
  try {
    const resp = await fetch(
      "https://restcountries.com/v3.1/all?fields=name,flags,capital,region,population,area,cca3",
    );
    const data = await resp.json();
    data.sort((a, b) => a.name.common.localeCompare(b.name.common));
    countries.value = data;
  } catch (err) {
    error.value = err.message;
  } finally {
    loading.value = false;
  }
};
onMounted(fetchCountries);

const filteredList = computed(() => {
  return countries.value.filter((country) => {
    const countryName = country.name.common.toLowerCase();
    return countryName.includes(search.value.toLowerCase());
  });
});
</script>
<template>
  <main>
    <header>
      <h1>{{ title }}</h1>
      <p>{{ subTitle }}</p>
    </header>
    <div class="search-container">
      <SearchBar class="search" v-model="search" />
      <p v-if="!loading && !error">{{ filteredList.length }} countries</p>
    </div>
    <div v-if="loading" class="state-message">
      <p>Loading countries...</p>
    </div>

    <div v-else-if="error" class="state-message">
      <p>Unable to load countries right now.</p>
      <button type="button" @click="fetchCountries">Reload</button>
    </div>

    <div v-else-if="filteredList.length === 0" class="state-message">
      <p>No countries match your search.</p>
    </div>

    <section v-else class="card-grid">
      <CountryCard
        v-for="country in filteredList"
        :key="country.cca3"
        :country="country"
      />
    </section>
  </main>
</template>
