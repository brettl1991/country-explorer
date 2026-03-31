<script setup>
const props = defineProps({
  country: Object,
});
</script>
<template>
  <article class="card">
    <img
      class="card-flag"
      :src="country.flags?.svg || country.flags?.png"
      :alt="`Flag of ${country.name.common}`"
      @error="
        ($event) => {
          $event.target.src = country.flags?.png || '';
        }
      "
    />

    <div class="card-body">
      <h2>{{ country.name.common }}</h2>
      <p>{{ country.region }}</p>
      <dl>
        <div>
          <dt>Capital</dt>
          <dd>{{ country.capital?.[0] || "N/A" }}</dd>
        </div>
        <div>
          <dt>Population</dt>
          <dd>{{ country.population.toLocaleString() }}</dd>
        </div>
        <div>
          <dt>Area</dt>
          <dd>{{ country.area.toLocaleString() }}</dd>
        </div>
      </dl>
      <button type="button">View details</button>
    </div>
  </article>
</template>
