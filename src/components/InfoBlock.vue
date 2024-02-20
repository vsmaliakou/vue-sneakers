<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String,
});
</script>

<template>
  <div class="flex flex-col items-center flex-1 justify-center w-72 mx-auto">
    <img
      :src="imageUrl"
      alt="img"
      height="120"
      width="120"
    >

    <h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>

    <p class="text-gray-400 text-center mt-2">{{ description }}</p>
  </div>
</template>
