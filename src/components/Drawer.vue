<script setup>
import DrawerHead from '@/components/DrawerHead.vue'
import CartItemList from '@/components/CartItemList.vue'
</script>

<template>
  <div class="fixed top-0 left-0 w-full h-full bg-slate-900 opacity-50 z-10"></div>
  <div class="bg-white w-96 h-full z-20 fixed right-0 top-0 p-8 flex flex-col justify-between">
    <DrawerHead/>
    <CartItemList/>

    <div class="flex flex-col gap-5 mt-7">
      <div class="flex gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed "></div>
        <b>12990 kzt.</b>
      </div>
      <div class="flex gap-2">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed "></div>
        <b>990 kzt.</b>
      </div>
      <button class=" mt-4 transition bg-lime-600 w-full rounded-xl py-3 text-white hover:bg-lime-700 active:bg-lime-800 disabled:bg-slate-300 cursor-pointer">Оформить заказ</button>
    </div>
  </div>
</template>

<style scoped>

</style>