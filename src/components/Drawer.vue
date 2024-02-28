<script setup>
import DrawerHead from '@/components/DrawerHead.vue'
import CartItemList from '@/components/CartItemList.vue'
import InfoBlock from '@/components/InfoBlock.vue'
import axios from 'axios'
import { computed, inject, ref } from 'vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
})
const isCreating = ref(false)
const orderId = ref(null)

const { cart, closeDrawer } = inject('cart')
const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post(
      `https://d67d1fa37781cbcd.mokky.dev/orders`, {
        items: cart.value,
        totalPrice: props.totalPrice.value
      }
    )

    cart.value = []

    orderId.value = data.id;
  } catch (e) {
    console.log(e)
  } finally {
    isCreating.value = false
  }
}

const cartIsEmpty = computed(() => cart.value.length === 0)
const cartButtonDisabled = computed(
  () => isCreating.value || cartIsEmpty.value
)

</script>

<template>
  <div class="fixed top-0 left-0 w-full h-full bg-slate-900 opacity-50 z-10"></div>
  <div class="bg-white w-96 h-full z-20 fixed right-0 top-0 p-8 flex flex-col">
    <DrawerHead/>

    <div class="flex h-full items-center" v-if="!totalPrice || orderId">
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пустая"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
        imageUrl="/package-icon.png"
      />
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен"
        :description="`Ваш номер заказа №${orderId}`"
        imageUrl="/order-success-icon.png"
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
          :disabled="cartButtonDisabled"
          @click="createOrder"
          class=" mt-4 transition bg-lime-600 w-full rounded-xl py-3 text-white hover:bg-lime-700 active:bg-lime-800 disabled:bg-slate-300 cursor-pointer">
          Оформить заказ
        </button>
      </div>
    </div>

  </div>
</template>

<style scoped>

</style>