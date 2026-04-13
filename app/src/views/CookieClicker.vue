<template>
  <div class="container">
    <h3 v-if="!loaded">Please wait for the API to load</h3>
    <h3>
      The 'census_tract' data value is present on the bar chart. It's just that the scale is in the
      thousands so for any values less than triple digits, its very hard to see.
    </h3>
    <PokemonCard v-for="touristsite in apiArray" :key="touristsite.bin" :city="touristsite" />
    <h3>
      For the chart below, smallRequests refer to requested funds under $3,500, mediumRequests
      between $3,500 and $7,500, and largeRequests being $7,500 and above for each Card
    </h3>
    <BarChart v-if="loaded" :data="allPokemonCardChartData" />
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
let loaded = ref(false)

let allPokemonCardChartData = ref({
  labels: ['smallRequest', 'mediumRequest', 'largeRequest'],
  datasets: [{ data: [] }], // Start empty, update after fetch
})

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
    loaded.value = true
    console.log(loaded.value)

    // Update chart data with fetched counts
    allPokemonCardChartData.value.datasets[0].data = [
      smallRequest.value,
      mediumRequest.value,
      largeRequest.value,
    ]
  } catch (error) {
    console.log(
      'There seems to be an error with this API iteration. Here is the error message: ' + error,
    )
  }
}
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
