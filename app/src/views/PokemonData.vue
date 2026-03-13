<template>
  <div>
    <h2>{{ pokemon.name }}</h2>
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from 'vue'
import { useRoute } from 'vue-router'
const route = useRoute()
const pokemon = ref(null)
async function getPokemon(id) {
  console.log('did i run?')
  const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${id}`)
  const data = await response.json()
  pokemon.value = data
}
watch(
  () => route.params.id,
  function (id) {
    getPokemon(id)
  },
)
onMounted(function () {
  getPokemon(route.params.id)
})
</script>

<style scoped></style>
