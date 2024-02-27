<script setup>
defineProps({
  totalPrice: Number,
})
const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header class="flex justify-between border-b border-slate-200 px-10 py-8">
    <div class="flex items-center gap-4">
      <img src="/logo.png" alt="Logo" class="w-10 h-auto">
      <div>
        <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
        <p class="text-slate-500">Best sneaker store online</p>
      </div>
    </div>
    <ul class="flex items-center gap-10">
      <li class="flex items-center gap-3" @click="emit('openDrawer')">
        <img src="/cart.svg" alt="cart">
        <span class="text-gray-500 hover:text-black cursor-pointer">{{ totalPrice }} kzt.</span>
      </li>
      <li class="flex items-center gap-3">
        <img src="/heart.svg" alt="cart">
        <span class="text-gray-500 hover:text-black cursor-pointer">Закладки</span>
      </li>
      <li class="flex items-center gap-3">
        <img src="/profile.svg" alt="cart">
        <span class="text-gray-500 hover:text-black cursor-pointer">Профиль</span>
      </li>
    </ul>
  </header>
</template>

<style scoped>

</style>