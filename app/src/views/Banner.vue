<template>
  <div class="container">
    <div class="stats">
      <p class="pulls">Pulls: {{ pulls }}</p>
      <p class="pity5">5★ pity: {{ pity5 }}</p>
      <p class="pity4">4★ pity: {{ pity4 }}</p>
      <p class="lastFeatured">Is the next 5★ Guaranteed: {{ lastfeatured ? 'No' : 'Yes' }}</p>
    </div>
    <button @click="reset">Reset</button>

    <div class="results">
      <h2>Results</h2>
      <ul>
        <li v-for="(char, i) in results" :key="i" :class="'rarity-' + char.rarity[0]">
          {{ char.name }} — {{ char.rarity }}
        </li>
      </ul>
    </div>
  </div>
  <img class="single-pull-button" @click="singlepull" src="/download (1).png" alt="" />
  <img class="ten-pull-button" @click="tenpull" src="/2.png" alt="" />
  <img class="Lingsha-banner" src="/Let_Scent_Sink_In_2024-10-02.webp" alt="Lingsha Banner" />
</template>

<script setup>
import { ref } from 'vue'

const featured5star = { name: 'Lingsha' }
const standard5star = [
  { name: 'Bronya' },
  { name: 'Welt' },
  { name: 'Himeko' },
  { name: 'Seele' },
  { name: 'Fu Xuan' },
  { name: 'Blade' },
  { name: 'Yanqing' },
  { name: 'Bailu' },
  { name: 'Gepard' },
  { name: 'Clara' },
]

const featured4star = [
  { name: 'Natasha', },
  { name: 'Misha', },
  { name: 'Guinaifen', },
]
const standard4star = [
  { name: 'Arlan', },
  { name: 'Asta', },
  { name: 'Dan Heng', },
  { name: 'Sampo', },
  { name: 'Herta', },
  { name: 'Gallagher', },
  { name: 'Hanya', },
  { name: 'Hook', },
  { name: 'Luka', },
  { name: 'Lynx', },
  { name: 'March 7th', },
  { name: 'Moze', },
  { name: 'Pela', },
  { name: 'Qingque', },
  { name: 'Serval', },
  { name: 'Susang', },
  { name: 'Yukong', },
  { name: 'Tingyun', },
  { name: 'Xueyi', },
]

const pulls = ref(0)
const pity5 = ref(0)
const pity4 = ref(0)
const lastfeatured = ref(true)
const results = ref([])

function roll5Star() {
  const isfeatured = lastfeatured.value === false ? true : Math.random() < 0.5
  lastfeatured.value = isfeatured
  return {
    name: isfeatured
      ? featured5star.name
      : standard5star[Math.floor(Math.random() * standard5star.length)].name,
    rarity: '5★',
  }
}

function roll4Star() {
  const pool = Math.random() < 0.5625 ? featured4star : standard4star
  return {
    name: pool[Math.floor(Math.random() * pool.length)].name,
    rarity: '4★',
  }
}

function gacha() {
  pulls.value++
  pity5.value++
  pity4.value++

  if (pity5.value >= 90) {
    pity5.value = 0
    pity4.value = 0
    return roll5Star()
  }

  if (pity4.value >= 10) {
    pity4.value = 0
    return roll4Star()
  }

  const r = Math.random()

  if (r < 0.006) {
    pity5.value = 0
    pity4.value = 0
    return roll5Star()
  }

  if (r < 0.056) {
    pity4.value = 0
    return roll4Star()
  }

  return { name: 'Random Light Cone', rarity: '3★' }
}

function singlepull() {
  results.value.unshift(gacha())
}

function tenpull() {
  for (let i = 0; i < 10; i++) {
    results.value.unshift(gacha())
  }
}

function reset() {
  pulls.value = 0
  pity5.value = 0
  pity4.value = 0
  lastfeatured.value = true
  results.value = []
}
</script>

<style scoped>
.container {
  max-width: 600px;
  margin: auto;
  padding: 20px;
}
.rarity-5 {
  color: gold;
  font-weight: bold;
}

.rarity-4 {
  color: violet;
  font-weight: bold;
}

.rarity-3 {
  color: gray;
}
.single-pull-button {
  position: absolute;
  bottom: 25px;
  right: 350px;
  max-width: 300px;
}
.ten-pull-button {
  position: absolute;
  bottom: 20px;
  right: 20px;
  max-width: 300px;
}
.container {
  position: absolute;
  top: 0px;
  left: 100px;
}
.Lingsha-banner {
  position: absolute;
  right: 0px;
  top: 0px;
  width: 1400px;
}
</style>
