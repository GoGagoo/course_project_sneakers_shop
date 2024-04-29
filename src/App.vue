<script setup>
import axios from 'axios'
import { onMounted, provide, reactive, ref, watch } from 'vue'

import CardList from './components/CardList.vue'
import Header from './components/Header.vue'
// import Drawer from './components/Drawer.vue'

const items = ref([])

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQuery = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://6f49ca9065638844.mokky.dev/favorites')

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

      if (!favorite) return item

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (error) {
    console.log(error)
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }

      item.isFavorite = true
      const { data } = await axios.post('https://6f49ca9065638844.mokky.dev/favorites', obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://6f49ca9065638844.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (error) {
    console.log(error)
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

    const { data } = await axios.get('https://6f49ca9065638844.mokky.dev/items', {
      params
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (error) {
    console.log(error)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})

watch(filters, fetchItems)

provide('addToFavorites', addToFavorite)
</script>

<template>
  <!-- <Drawer /> -->
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

        <div class="flex gap-4">
          <select
            @change="onChangeSelect"
            class="py-2 px-3 border rounded-md outline-none"
            name=""
            id=""
          >
            <option value="name">По названию</option>
            <option value="price">По дешевой цене</option>
            <option value="-price">По дорогой цене</option>
          </select>

          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="" />
            <input
              @change="onChangeSearchInput"
              class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
              placeholder="Поиск"
              type="text"
            />
          </div>
        </div>
      </div>
    </div>

    <div class="mt-10">
      <CardList :items="items" @addToFavorite="addToFavorite" />
    </div>
  </div>
</template>
