<script setup>
import { inject, onMounted, reactive, ref, watch } from 'vue';
import axios from 'axios';
import debounce from 'lodash.debounce';

import CardList from '../CardList.vue';

const cartItems = inject('cartItems');
const toggleIsAdded = inject('toggleIsAdded');

const items = ref([]);
const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
});

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value;
};

const onChangeSearch = debounce((event) => {
  filters.searchQuery = event.target.value;
}, 500);

const fetchProducts = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    };

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`;
    }

    const response = await axios.get(`https://65b899cb2066d83e.mokky.dev/products`, { params });
    items.value = response.data.map((product) => ({
      ...product,
      isFavorite: false,
      favoriteId: null,
      isAdded: false,
    }));
  } catch (error) {
    console.log(error);
  }
}

const fetchFavorites = async () => {
  try {
    const response = await axios.get(`https://65b899cb2066d83e.mokky.dev/favorites`);
    items.value = items.value.map((item) => {
      const favorite = response.data.find((favoriteItem) => favoriteItem.product_id === item.id);

      return favorite
        ? {
            ...item,
            isFavorite: true,
            favoriteId: favorite.id,
          }
        : item;
    });
  } catch (error) {
    console.log(error);
  }
}

const toggleIsFavorite = async (product) => {
  try {
    if (!product.isFavorite) {
      product.isFavorite = true;
      const response = await axios.post(`https://65b899cb2066d83e.mokky.dev/favorites`, { product_id: product.id });
      product.favoriteId = response.data.id;
    } else {
      product.isFavorite = false;
      await axios.delete(`https://65b899cb2066d83e.mokky.dev/favorites/${product.favoriteId}`);
      product.favoriteId = null;
    }
  } catch (error) {
    console.log(error);
  }
};

onMounted(async () => {
  const localStorageCart = localStorage.getItem('cart');
  cartItems.value = localStorageCart ? JSON.parse(localStorageCart) : [];

  await fetchProducts();
  await fetchFavorites();

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cartItems.value.some((cartItem) => cartItem.id === item.id),
  }));
});

watch(filters, fetchProducts);
watch(cartItems, () => {
  items.value = items.value.map((item) => ({ ...item, isAdded: false }));
});
</script>

<template>
  <div class="flex justify-between items-center mb-8">
    <h2 class="text-3xl font-bold">Все кроссовки</h2>

    <div class="flex items-center gap-4">
      <select
        class="py-2 px-3 border rounded-md outline-none"
        @change="onChangeSelect"
      >
        <option value="title">По названию</option>
        <option value="price">По цене (дешевые)</option>
        <option value="-price">По цене (дорогие)</option>
      </select>

      <div class="relative">
        <img
          src="/search.svg"
          alt="Search"
          class="absolute left-4 top-3"
        >

        <input
          type="text"
          placeholder="Поиск..."
          class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
          @input="onChangeSearch"
        >
      </div>
    </div>
  </div>

  <CardList
    :items="items"
    @toggle-is-favorite="toggleIsFavorite"
    @toggle-is-added="toggleIsAdded"
  />
</template>
