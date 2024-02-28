<script setup>

import CardList from '@/components/CardList.vue'
import axios from 'axios'
import debounce from 'lodash.debounce'
import { inject, onMounted, reactive, ref, watch } from 'vue'

const { cart, addToCart, removeFromCart } = inject('cart')

const items = ref([])

const filters = reactive({
  sortBy: 'name',
  searchQuery: ''
})

const addToFavorite = async (item) => {
  try {
    item.isFavorite = !item.isFavorite;
    let favoriteId = item.favoriteId;

    if (item.isFavorite) {
      const obj = {
        item_id: item.id
      };
      const { data } = await axios.post(
        `https://d67d1fa37781cbcd.mokky.dev/favorites`,
        obj
      );
      item.favoriteId = data.id;
    } else if (favoriteId) {
      await axios.delete(
        `https://d67d1fa37781cbcd.mokky.dev/favorites/${favoriteId}`
      );
      item.favoriteId = null;
    }
  } catch (e) {
    console.log(e);
  }
};
const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value
}

const onChangeSearchInput = debounce((e) => {
  filters.searchQuery = e.target.value
},500 )

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://d67d1fa37781cbcd.mokky.dev/favorites`
    )
    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.item_id === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (e) {
    console.log(e)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get(
      `https://d67d1fa37781cbcd.mokky.dev/items`,
      { params }
    )
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (e) {
    console.log(e)
  }
}


onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : [];

  await fetchItems()


  await fetchFavorites()
  console.log(items.value)
  items.value = items.value.map(item => ({
    ...item,
    isAdded: cart.value.some(cartItem => cartItem.id === item.id)
  }))
})

watch(cart, () => {
  items.value = items.value.map(item => ({
    ...item,
    isAdded: false
  }))
})
watch(filters, fetchItems)

</script>

<template>
  <div class="flex justify-between items-center" >
    <h2 class="text-3xl font-bold mb-8">All sneakers</h2>

    <div class="flex gap-4">
      <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none bg-white">
        <option value="name">По названию</option>
        <option value="price">По цене (дешевые)</option>
        <option value="-price">По цене (дорогие)</option>
      </select>
      <div class="relative">
        <img class="absolute left-4 top-3" src="/search.svg" alt="">
        <input @input="onChangeSearchInput" type="text" placeholder="Поиск"
               class="border rounded-md pl-11 py-2 pr-4 outline-none  focus:border-gray-400 ">
      </div>
    </div>
  </div>
  <div class="mt-10">
    <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
  </div>
</template>

<style scoped>

</style>