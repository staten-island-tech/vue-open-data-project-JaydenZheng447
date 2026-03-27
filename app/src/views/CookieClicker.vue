<template>
  <div class="container">
    <PokemonCard v-for="touristsite in pokemon" :key="touristsite.bin" :city="touristsite" />
    <BarChart />
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

const pokemon = ref([])
let smallRequest = ref(0)
let mediumRequest = ref(0)
let largeRequest = ref(0)

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
      'I have numManhattan of ' +
        smallRequest.value +
        ' and ' +
        mediumRequest.value +
        ' and ' +
        largeRequest.value,
    )
    pokemon.value = data
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

// console.log(pokemon.value)
// for (let i = 0; i < pokemon.length; i++) {
//   if (pokemon[i].borough === 'MANHATTAN') {
//     numManhattan.value += 1
//   } else if (pokemon[i].borough === 'BsROOKLYN') {
//     numBrooklyn.value += 1
//   }
// }
//
// console.log('ur number of brooklyns is ' + numBrooklyn)
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
