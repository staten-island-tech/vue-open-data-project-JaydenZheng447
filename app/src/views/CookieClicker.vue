<template>
  <div class="container">
    <h3 v-if="loaded = false">Please wait for the API to load</h3>
    <PokemonCard v-for="touristsite in apiArray" :key="touristsite.bin" :city="touristsite" />
    <BarChart v-if="loaded" />
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
let loaded = false

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
    apiArray.value = data
    loaded = true
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
