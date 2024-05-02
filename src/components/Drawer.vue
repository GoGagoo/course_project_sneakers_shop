<script setup>
import CardItemList from './CartItemList.vue'
import DrawerHead from './DrawerHead.vue'
import InfoBlock from './InfoBlock.vue'

defineProps({
  totalPrice: Number,
  vatPrice: Number,
  buttonDisabled: Boolean
})

const emit = defineEmits(['createOrder'])
</script>

<template>
  <div class="fixed z-10 top-0 h-full w-full bg-black opacity-70" />
  <div
    class="flex flex-col justify-between fixed z-10 top-0 h-full right-0 w-96 bg-white px-10 py-7 overflow-y-auto"
  >
    <DrawerHead />
    <InfoBlock
      v-if="!totalPrice"
      title="Корзина пустая"
      description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
      image-url="/package-icon.png"
    />
    <CardItemList />

    <div v-if="totalPrice" class="flex flex-col gap-4 mt-7">
      <div class="flex items-end gap-2">
        <span>Итого:</span>
        <div class="flex-1 border-b border-dashed" />
        <span class="font-bold">{{ totalPrice }}</span>
      </div>

      <div class="flex items-end gap-2">
        <span>Налог 5%:</span>
        <div class="flex-1 border-b border-dashed" />
        <span class="font-bold">{{ vatPrice }}</span>
      </div>
      <button
        :disabled="buttonDisabled"
        @click="() => emit('createOrder')"
        class="flex justify-center mt-4 disabled:bg-slate-400 items-center gap-3 w-full py-3 bg-lime-500 text-white rounded-xl transition active:bg-lime-700 hover:bg-lime-600"
      >
        Оформить заказ
        <img src="/arrow-next.svg" alt="Arrow" />
      </button>
    </div>
  </div>
</template>
