<template>
  <div class="card">
    <router-link :to="pkmnPath">
      <h3>Bin: {{ props.city.bin }}</h3>
    </router-link>
    <h3>Name: {{ props.city.organization_name }}</h3>
    <h3>Address: {{ props.city.organization_address }}</h3>
    <h3>Borough: {{ props.city.borough }}</h3>
    <BarChart :data="chartData2" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { computed } from 'vue'
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

const binVar = computed(() => {
  return props.city.bin
})
console.log(binVar)

let chartData2 = {
  labels: ['amount_requested', 'bin', 'census_tract'],
  datasets: [{ data: [400, 200, 120] }],
}

const props = defineProps({
  city: {
    type: Object,
    required: true,
  },
})
const pkmnPath = computed(() => {
  return `/pkmn/${props.city.bin}`
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
  text-align: center;
}

img {
  width: 100%;
  height: 80%;
  object-fit: cover;
}
h3 {
  display: inline-block;
}
</style>
