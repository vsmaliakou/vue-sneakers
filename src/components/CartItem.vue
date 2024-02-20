<script setup>
defineProps({
  item: Object,
});

const emit = defineEmits(['toggleIsAdded']);
</script>

<template>
  <div class="flex items-center border border-slate-200 p-4 rounded-xl gap-4">
    <img
      class="w-16"
      :src="item.imageUrl"
      alt="Sneaker"
    >

    <div class="flex flex-col flex-1">
      <p>{{ item.title }}</p>

      <div class="flex justify-between mt-2">
        <b>{{ item.price }} руб.</b>

        <img
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
          src="/close.svg"
          alt="Close"
          @click="emit('toggleIsAdded')"
        >
      </div>
    </div>
  </div>
</template>
