<template>
  <div class="container">
    <h1>HSR Banner</h1>

    <div class="stats">
      <p>Pulls: {{ pulls }}</p>
      <p>5★ pity: {{ pity5 }}</p>
      <p>4★ pity: {{ pity4 }}</p>
      <p>Last 5★ was featured: {{ lastFeatured ? "Yes" : "No" }}</p>
    </div>

    <button @click="singlepull">Pull 1</button>
    <button @click="tenpull">Pull 10</button>
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
</template>

<script setup>
import { ref } from "vue"


const featured5star = "Lingsha"
const standard5star = ["Bronya", "Welt", "Himeko", "Seele", "Fu Xuan", "Blade", "Yanqing", "Bailu", "Gepard", "Clara"]

const featured4star = ["Natasha", "Misha", "Guinaifen"]
const standard4star = ["Arlan", "Asta", "Dan Heng", "Sampo", "Herta", "Gallagher", "Hanya", "Hook", "Luka", "Lynx", "March 7th", "Moze", "Pela", "Qingque", "Serval", "Susang", "Yukong", "Tingyun","Xueyi"]

const pulls = ref(0)
const pity5 = ref(0)
const pity4 = ref(0)
const lastfeatured = ref(true)
const results = ref([])

function roll5Star() {
  const isfeatured = lastfeatured.value ? true : Math.random() < 0.5
  lastfeatured.value = isfeatured
  return {
    name: isfeatured ? featured5star : standard5star[Math.floor(Math.random() * standard5star.length)],
    rarity: "5★"
  }
}

function roll4Star() {
  const pool = Math.random() < 0.5 ? featured4star : standard4star
  return {
    name: pool[Math.floor(Math.random() * pool.length)],
    rarity: "4★"
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

  return { name: "Random Light Cone", rarity: "3★" }
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
</style>
