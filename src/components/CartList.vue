<script setup>
import { inject } from 'vue';
import CartItem from './CartItem.vue';

const cartItems = inject('cartItems');
const toggleIsAdded = inject('toggleIsAdded');
</script>

<template>
  <div class="flex flex-col flex-1 gap-4" v-auto-animate>
    <CartItem
      v-for="item in cartItems"
      :key="item.id"
      :item="item"
      @toggle-is-added="() => toggleIsAdded(item)"
    />
  </div>
</template>