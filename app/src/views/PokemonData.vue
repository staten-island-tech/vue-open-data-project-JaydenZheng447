<template>
  <div>
    <h2>Bin: {{ pokemon?.data?.id }}</h2>
    <p>Organization: {{ pokemon?.organization_name }}</p>
    <p>Address: {{ pokemon?.organization_address }}</p>
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from 'vue'
import { useRoute } from 'vue-router'
const route = useRoute()
const pokemon = ref(null)
async function getPokemon(bin) {
  try {
    console.log('did i run? here`s the bin: ' + bin)
    const response = await fetch(`https://data.cityofnewyork.us/resource/x4ud-jhxu.json?bin=${bin}`)
    const data = await response.json()
    pokemon.value = data
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
</script>

<style scoped></style>
