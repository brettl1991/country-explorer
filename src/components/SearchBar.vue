<script setup>
const props = defineProps({
  search: String,
});
const emit = defineEmits(["update-search"]);
</script>
<template>
  <form @submit.prevent>
    <label for="search-input">Search countries</label>
    <input
      type="search"
      id="search-input"
      placeholder="Search by country name"
      :value="search"
      @input="emit('update-search', $event.target.value)"
    />
  </form>
</template>
