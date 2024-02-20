<script setup>
defineProps({
  item: Object,
  isFavorites: Boolean,
});

const emit = defineEmits(['toggleIsFavorite', 'toggleIsAdded']);
</script>

<template>
  <div
    class="relative
      bg-white
      border
      border-slate-100
      rounded-3xl
      p-8
      cursor-pointer
      transition
      hover:-translate-y-2
      hover:shadow-xl
      flex
      flex-col"
  >
    <img
      v-if="!isFavorites"
      :src="item.isFavorite ? '/like-2.svg' : '/like-1.svg'"
      alt="Like"
      class="absolute top-8 left-8"
      @click="emit('toggleIsFavorite')"
    >

    <img :src="item.imageUrl" alt="Sneaker">

    <p class="mt-2 mb-auto">{{ item.title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>

        <b>{{ item.price }} руб.</b>
      </div>

      <img
        v-if="!isFavorites"
        :src="item.isAdded ? '/checked.svg' : '/plus.svg'"
        alt="Plus"
        @click="emit('toggleIsAdded')"
      >
    </div>
  </div>
</template>
