<script setup>
import { computed, onMounted, provide, reactive, ref, watch } from 'vue'
import axios from 'axios'
import Header from '@/components/Header.vue'
import CardList from '@/components/CardList.vue'
import Drawer from '@/components/Drawer.vue'

const items = ref([])
const cart = ref([])
const isCreatingOrder = ref(false)

const filters = reactive({
  sortBy: 'name',
  searchQuery: ''
})

const drawerOpen = ref(false)

const totalPrice = computed(
  () => cart.value.reduce((acc, item) => acc + item.price, 0)
)

const vatPrice = computed(
  () => Math.round(totalPrice.value * 0.05)
)
const cartIsEmpty = computed(() => cart.value.length === 0)
const cartButtonDisabled = computed(
  () => isCreatingOrder.value || cartIsEmpty.value
)

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}
const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post(
      `https://d67d1fa37781cbcd.mokky.dev/orders`, {
        items: cart.value,
        totalPrice: totalPrice.value
      }
    )

    cart.value = []

    return data
  } catch (e) {
    console.log(e)
  } finally {
    isCreatingOrder.value = false
  }
}

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

const onChangeSearchInput = (e) => {
  filters.searchQuery = e.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(
      `https://d67d1fa37781cbcd.mokky.dev/favorites`
    )
    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.parentId === item.id)

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

const addToFavorite = async (item) => {
  try {
    item.isFavorite = !item.isFavorite
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }
      const { data } = await axios.post(
        `https://d67d1fa37781cbcd.mokky.dev/favorites`,
        obj
      )
      item.favoriteId = data.id
      console.log(data)
    } else {
      await axios.delete(
        `https://d67d1fa37781cbcd.mokky.dev/favorites/${item.favoriteId}`
      )
      item.favoriteId = null
    }
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

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}


onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : [];

  await fetchItems()

  await fetchFavorites()
  items.value = items.value.map(item => ({
    ...item,
    isAdded: cart.value.some(cartItem => cartItem.id === item.id)
  }))
})
watch(filters, fetchItems)

watch(cart, () => {
  items.value = items.value.map(item => ({
    ...item,
    isAdded: false
  }))
})
watch(cart, () => {
  localStorage.setItem('cart', JSON.stringify(cart.value))
}, { deep: true })
provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  removeFromCart
})
</script>
<template>
  <Drawer
    v-if="drawerOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    :button-disabled="cartButtonDisabled"
    @create-order="createOrder"
  />
  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <Header :total-price="totalPrice" @open-drawer="openDrawer" />
    <div class="p-10">
      <div class="flex justify-between items-center">
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
    </div>
  </div>
</template>
