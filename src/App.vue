<script setup>
import axios from 'axios'
import { onMounted, reactive, ref, watch } from 'vue'

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

const onChangeSearchInput = () => {
  filters.searchQuery = event.target.value
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    }
    
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    
    const { data } = await axios.get('https://6f49ca9065638844.mokky.dev/items', {
      params
    })

    items.value = data
  } catch (error) {
    console.log(error)
  }
}

onMounted(fetchItems)
watch(filters, fetchItems)
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
      <CardList :items="items" />
    </div>
  </div>
</template>
