<script setup>
import axios from 'axios';
import { onMounted, ref } from 'vue';

import CardList from '../CardList.vue';

const favorites = ref([]);

onMounted(async () => {
  try {
    const response = await axios.get(`https://65b899cb2066d83e.mokky.dev/favorites?_relations=products`);
    favorites.value = response.data.map((item) => ({
      favorite_id: item.id,
      ...item.product,
    }));
  } catch (error) {
    console.log(error);
  }
});
</script>

<template>
  <div class="flex justify-between items-center mb-8">
    <h2 class="text-3xl font-bold">Мои закладки</h2>
  </div>

  <CardList
    :items="favorites"
    is-favorites
  />
</template>
