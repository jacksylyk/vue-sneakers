<script setup>
import Card from '@/components/Card.vue'
defineProps({
  items: Array,
})
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card v-for="item in items" :key="item.id" :image-url="item.imageUrl" :title="item.title" :price="item.price"></Card>
  </div>
</template>

<style scoped>

</style>