<template>
  <div>
    <h2>Bin: {{ pokemon?.bin }}</h2>
    <p>Organization: {{ pokemon?.organization_name }}</p>
    <p>Address: {{ pokemon?.organization_address }}</p>
    <BarChart :data="chartData3" />
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from 'vue'
import { useRoute } from 'vue-router'

import { computed } from 'vue'
const route = useRoute()
const pokemon = ref(null)
async function getPokemon(bin) {
  try {
    console.log('did i run? here`s the bin: ' + bin)
    const response = await fetch(`https://data.cityofnewyork.us/resource/x4ud-jhxu.json?bin=${bin}`)
    const data = await response.json()
    pokemon.value = data[0]
  } catch (error) {
    console.log(error)
  }
}
watch(
  () => route.params.bin,
  function (bin) {
    getPokemon(bin)
  },
)
onMounted(function () {
  getPokemon(route.params.bin)
})

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

import BarChart from '../views/BarChart.vue'

const chartData3 = computed(() => ({
  labels: ['amount_requested', 'bin', 'census_tract'],
  datasets: [
    {
      data: [
        Number(pokemon?.amount_requested) || 0,
        Number(pokemon?.zip) || 0,
        Number(pokemon?.census_tract) || 0,
      ],
    },
  ],
}))
</script>

<style scoped></style>
