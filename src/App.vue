<script setup>
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
import Drawer from '@/components/Drawer.vue'
</script>
<template>
<!--  <Drawer/>-->
  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <Header />
    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">All sneakers</h2>

        <div class="flex gap-4">
          <select class="py-2 px-3 border rounded-md outline-none bg-white">
            <option value="1">По названию</option>
            <option value="2">По цене (дешевые)</option>
            <option value="3">По цене (дорогие)</option>
          </select>
          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="">
            <input type="text" placeholder="Поиск" class="border rounded-md pl-11 py-2 pr-4 outline-none  focus:border-gray-400 ">
          </div>
        </div>
      </div>
      <CardList></CardList>
    </div>
  </div>
</template>
