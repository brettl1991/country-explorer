<script setup>
import { onMounted, ref } from "vue";
import CountryCard from "./components/CountryCard.vue";
import SearchBar from "./components/SearchBar.vue";

const countries = ref([]);
const search = ref("");
const loading = ref(false);
const error = ref(null);

const fetchCountries = async () => {
  loading.value = true;
  error.value = null;
  try {
    const resp = await fetch(
      "https://restcountries.com/v3.1/all?fields=name,flags,capital,region,population,area,cca3",
    );
    const data = await resp.json();
    console.log(data);
    countries.value = data;
  } catch (err) {
    error.value = err.message;
  } finally {
    loading.value = false;
  }
};

onMounted(fetchCountries);
</script>
<template>
  <main>
    <header>
      <h1></h1>
      <p></p>
    </header>
    <div class="search-container">
      <SearchBar class="search" />
      <p>Resoultcount</p>
    </div>
    <p></p>
    <section class="card-grid">
      <CountryCard class="card" />
    </section>
  </main>
</template>
