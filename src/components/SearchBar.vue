<script setup>
const props = defineProps({
  modelValue: String,
});
const emit = defineEmits(["update:modelValue"]);
</script>
<template>
  <form @submit.prevent>
    <label for="search-input">Search countries</label>
    <input
      type="search"
      id="search-input"
      placeholder="Search by country name"
      :value="modelValue"
      @input="emit('update:modelValue', $event.target.value)"
    />
  </form>
</template>
