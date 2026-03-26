<template>
  <div class="card">
    <router-link :to="pkmnPath">
      <h3>Bin: {{ props.pokemon.bin }}</h3>
    </router-link>
    <h3>Name: {{ props.pokemon.organization_name }}</h3>
    <h3>Address: {{ props.pokemon.organization_address }}</h3>
    <h3>Borough: {{ props.pokemon.borough }}</h3>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { computed } from 'vue'
const props = defineProps({
  pokemon: {
    type: Object,
    required: true,
  },
})
const pkmnPath = computed(() => {
  return `/pkmn/${props.pokemon.bin}`
})

// let numManhattan = ref(0)
// let numBrooklyn = ref(0)
// for (let i = 0; i < props.pokemon.length; i++) {
//   if (props.pokemon.borough === 'MANHATTAN') {
//     numManhattan.value += 1
//   } else if (props.pokemon.borough === 'BROOKLYN') {
//     numBrooklyn.value += 1
//   }
// }

// console.log('ur number of brooklyns is ' + numBrooklyn)
// console.log('ur number of manhattans is ' + numManhattan)
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
