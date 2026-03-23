<template>
  <div class="card">
    <h2>{{ arrests.ofns_desc }}</h2>
    <h3>{{ arrests.pd_desc }}</h3>
    <p>race: {{ arrests.perp_race }}</p>
    <p>gender: {{ arrests.perp_sex }}</p>
    <p>age:{{ arrests.age_group }}</p>
  </div>
</template>

<script setup>
const props = defineProps({
  arrests: {
    type: Object,
    required: true,
  },
})
</script>

<style scoped>
.card {
  width: 28%;
  height: 500px;
  background-color: aliceblue;
  margin: 30px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 10px;
  text-transform: uppercase;
}
img {
  width: 100%;
  height: 80%;
  object-fit: cover;
}
</style>