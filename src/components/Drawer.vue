<script setup>
import DrawerHead from '@/components/DrawerHead.vue'
import CartItemList from '@/components/CartItemList.vue'
import { computed } from 'vue'
import InfoBlock from '@/components/InfoBlock.vue'

defineProps({
  totalPrice: Number,
  vatPrice: Number,
  buttonDisabled: Boolean,
})

const emit = defineEmits(['createOrder'])


</script>

<template>
  <div class="fixed top-0 left-0 w-full h-full bg-slate-900 opacity-50 z-10"></div>
  <div class="bg-white w-96 h-full z-20 fixed right-0 top-0 p-8 flex flex-col">
    <DrawerHead/>
    <div class="flex h-full items-center" v-if="!totalPrice">
      <InfoBlock
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        imageUrl="/package-icon.png"

      />
    </div>
    <div v-else class="flex flex-col flex-1 justify-between">
      <CartItemList/>

      <div class="flex flex-col gap-5 mt-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed "></div>
          <b>{{ totalPrice }} kzt.</b>
        </div>
        <div class="flex gap-2">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed "></div>
          <b>{{ vatPrice }} kzt.</b>
        </div>
        <button
          @click="() => emit('createOrder')"
          class=" mt-4 transition bg-lime-600 w-full rounded-xl py-3 text-white hover:bg-lime-700 active:bg-lime-800 disabled:bg-slate-300 cursor-pointer"
          :disabled="buttonDisabled">
          Оформить заказ
        </button>
      </div>
    </div>

  </div>
</template>

<style scoped>

</style>