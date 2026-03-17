<template>
  <div class="container">
    <PokemonCard
      v-for="mon in pokemon"
      :key="mon.name"
      :pokemon="mon"
      :organization_address="mon.organization_address"
      :bin="mon.bin"
    />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

import PokemonCard from '../components/PokemonCard.vue'
const pokemon = ref([])
async function getPokemon() {
  try {
    console.log('fetched')
    const response = await fetch('https://data.cityofnewyork.us/resource/x4ud-jhxu.json')
    const data = await response.json()
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
