<script setup>
import { computed, provide, ref, watch } from 'vue';

import Header from './components/Header.vue';
import Drawer from './components/Drawer.vue';

const cartItems = ref([]);
const openDrawer = ref(false);
const totalPrice = computed(() => cartItems.value.reduce((acc, item) => acc + item.price, 0));

const toggleIsAdded = (product) => {
  if (!product.isAdded) {
    cartItems.value.push(product);
    product.isAdded = true;
  } else {
    cartItems.value.splice(cartItems.value.indexOf(product), 1);
    product.isAdded = false;
  }
};

const toggleDrawerVisability = () => {
  openDrawer.value = !openDrawer.value
};

watch(cartItems, () => {
  localStorage.setItem('cart', JSON.stringify(cartItems.value));
}, { deep: true });

provide('cartItems', cartItems);
provide('toggleDrawerVisability', toggleDrawerVisability);
provide('toggleIsAdded', toggleIsAdded);
</script>

<template>
  <Drawer
    v-if="openDrawer"
    :total-price="totalPrice"
  />

  <div
    class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14"
  >
    <Header
      :total-price="totalPrice"
      @toggle-drawer-visability="toggleDrawerVisability"
    />

    <div class="p-10">
      <router-view></router-view>
    </div>
  </div>
</template>
