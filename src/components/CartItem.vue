<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
})

const emit = defineEmits(['onClickRemove'])
</script>

<template>
  <div class="border border-slate-200 p-4 rounded-xl flex items-center gap-5 w">
    <img  class="w-1/4 h-auto" :src="imageUrl" :alt="title">
    <div class="flex flex-col w-3/4">
      <p >{{ title }}</p>
      <div class="flex justify-between mt-2 ">
        <b>{{ price }} kzt.</b>
        <img @click="emit('onClickRemove')" class="opacity-40 hover:opacity-100 cursor-pointer transition" src="/close.svg" alt="Close">
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>