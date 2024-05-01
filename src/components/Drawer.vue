<script setup>
import CardItemList from './CartItemList.vue'
import DrawerHead from './DrawerHead.vue'
// overflow-y-auto for 2nd class 
</script>

<template>
  <div class="fixed z-10 top-0 h-full w-full bg-black opacity-70" />
  <div
    class="flex flex-col justify-between fixed z-10 top-0 h-full right-0 w-96 bg-white px-10 py-7"
  >
    <DrawerHead />
    <CardItemList />

    <div class="flex flex-col gap-4 mt-7">
      <div class="flex items-end gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed" />
        <span class="font-bold">1000 руб.</span>
      </div>

      <div class="flex items-end gap-2">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed" />
        <span class="font-bold">50 руб.</span>
      </div>
      <button
        disabled=""
        class="flex justify-center mt-4 disabled:bg-slate-400 items-center gap-3 w-full py-3 bg-lime-500 text-white rounded-xl transition active:bg-lime-700 hover:bg-lime-600"
      >
        Оформить заказ
        <img src="/arrow-next.svg" alt="Arrow" />
      </button>
    </div>
  </div>
</template>
