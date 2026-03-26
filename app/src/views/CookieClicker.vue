<template>
  <div class="container">
    <PokemonCard v-for="touristsite in pokemon" :key="touristsite.bin" :pokemon="touristsite" />
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

async function getPokemon() {
  try {
    console.log('fetched')
    const response = await fetch('https://data.cityofnewyork.us/resource/x4ud-jhxu.json')
    const data = await response.json()
    pokemon.value = data
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
