<script setup>
import Card from '@/components/Card.vue'
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card image-url="sneakers/sneakers-1.jpg" title="Sneaker 1" :price="60000" :is-added="false"
          :is-favorite="true" :on-click-add="onClickAdd"></Card>
  </div>
</template>

<style scoped>

</style>