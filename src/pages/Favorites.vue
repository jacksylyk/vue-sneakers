<script setup>

import { onMounted, ref } from 'vue'
import axios from 'axios'
import CardList from '@/components/CardList.vue'

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      `https://d67d1fa37781cbcd.mokky.dev/favorites?_relations=items`
    )
    favorites.value = data.map(obj => obj.item);
  } catch (e) {
    console.log(e)
  }
})



</script>

<template>
  <h2 class="text-3xl font-bold mb-8" >Favorites</h2>
  <div class="mt-10">
    <CardList :items="favorites" is-favorites/>
  </div>
</template>

<style scoped>

</style>