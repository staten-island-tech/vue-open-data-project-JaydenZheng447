<template>
  <div class="container">
<<<<<<< HEAD
    <h3 v-if="loaded = false">Please wait for the API to load</h3>
    <PokemonCard v-for="touristsite in pokemon" :key="touristsite.bin" :city="touristsite" />
    <BarChart v-if="loaded" />
=======
    <PokemonCard v-for="touristsite in apiArray" :key="touristsite.bin" :city="touristsite" />
    <BarChart v-if="loaded" :data="chartData" :options="chartOptions" />
>>>>>>> f3ac9ad56b3fd1164c2b6ffc971f617ecad1f9f2
  </div>
</template>
<!-- 
:organization_address="mon.organization_address"
      :bin="mon.bin" -->

<script setup>
import { ref, onMounted } from 'vue'
import { Bar } from 'vue-chartjs'
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
} from 'chart.js'

import PokemonCard from '../components/PokemonCard.vue'
import BarChart from './BarChart.vue'

import { computed } from 'vue'

const apiArray = ref([])
let smallRequest = ref(0)
let mediumRequest = ref(0)
let largeRequest = ref(0)
<<<<<<< HEAD
let loaded = false

=======
const loaded = ref(false)
>>>>>>> f3ac9ad56b3fd1164c2b6ffc971f617ecad1f9f2
async function getPokemon() {
  try {
    console.log('fetched')
    const response = await fetch('https://data.cityofnewyork.us/resource/x4ud-jhxu.json')
    const data = await response.json()
    console.log(data)
    console.log('length is ' + data.length)
    for (let i = 0; i < data.length; i++) {
      if (data[i].amount_requested <= 3500) {
        smallRequest.value += 1
      } else if (data[i].amount_requested <= 7500) {
        mediumRequest.value += 1
      } else if (data[i].amount_requested <= 10000) {
        largeRequest.value += 1
      }
    }

    console.log(
      'I have S-M-L request value types with counts of ' +
        smallRequest.value +
        ' and ' +
        mediumRequest.value +
        ' and ' +
        largeRequest.value,
    )
<<<<<<< HEAD
    pokemon.value = data
    loaded = true
=======
    apiArray.value = data
>>>>>>> f3ac9ad56b3fd1164c2b6ffc971f617ecad1f9f2
  } catch (error) {
    console.log(error)
  }
}

//https://pokeapi.co/api/v2/pokemon?limit=151&offset=0
//https://data.cityofnewyork.us/resource/sejx-2gn3.json
//https://data.cityofnewyork.us/resource/x4ud-jhxu.json

onMounted(() => {
  getPokemon()
})
</script>

<style scoped>
.container {
  width: 80vw;
  margin: 30px auto;
  display: flex;
  flex-wrap: wrap;
  flex-direction: row;
  align-items: center;
  justify-content: space-around;
}
</style>
