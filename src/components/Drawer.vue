<script setup>
import DrawerHead from './DrawerHead.vue';
import CartList from './CartList.vue';
import InfoBlock from './InfoBlock.vue';
import axios from 'axios';
import { ref, inject, computed } from 'vue';

const { totalPrice } = defineProps({
  totalPrice: Number,
});

const cartItems = inject('cartItems');
const isLoading = ref(false);
const orderId = ref(null);
const cartIsEmpty = computed(() => !cartItems.value.length);
const vatPrice = computed(() => totalPrice * 0.05);

const createOrder = async () => {
  try {
    isLoading.value = true;
    const response = await axios.post(`https://65b899cb2066d83e.mokky.dev/orders`, {
      items: cartItems.value,
      totalPrice,
    });

    cartItems.value = [];
    orderId.value = response.data.id;
  } catch (error) {
    console.log(error);
  } finally {
    isLoading.value = false;
  }
};
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>  
  <div class="bg-white w-96 h-full fixed -right-0 top-0 z-20 p-8 flex flex-col">
    <DrawerHead />

    <InfoBlock
      v-if="orderId"
      title="Заказ оформлен!"
      :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
      image-url="./order-success-icon.png"
    />

    <InfoBlock
      v-else-if="!totalPrice && !orderId"
      title="Корзина пустая"
      description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
      image-url="./package-icon.png"
    />

    <div
      v-else
      class="flex flex-col flex-1"
    >
      <CartList />
  
      <div class="flex flex-col gap-4 mt-7">
        <div class="flex gap-2">
          <span>Итого:</span>
  
          <div class="flex-1 border-b border-dashed"></div>
  
          <b>{{ totalPrice }} руб.</b>
        </div>
  
        <div class="flex gap-2">
          <span>Налог 5%:</span>
  
          <div class="flex-1 border-b border-dashed"></div>
  
          <b>{{ vatPrice }} руб.</b>
        </div>
  
        <button
          class="transition
            bg-lime-500
            w-full
            rounded-xl
            py-3
            text-white
            hover:bg-lime-600
            active:bg-lime-700
            cursor-pointer
            disabled:bg-slate-300
            mt-4"
          :disabled="isLoading || cartIsEmpty"
          @click="createOrder"
        >
          {{ isLoading ? 'Оформление заказа' : 'Оформить заказ'}}
        </button>
      </div>
    </div>
  </div>
</template>
