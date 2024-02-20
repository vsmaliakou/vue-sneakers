<script setup>
import Card from './Card.vue';

defineProps({
  items: Array,
  isFavorites: Boolean,
});

const emit = defineEmits(['toggleIsFavorite', 'toggleIsAdded']);
</script>

<template>
  <div class="grid grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :item="item"
      :is-favorites="isFavorites"
      @toggle-is-added="emit('toggleIsAdded', item)"
      @toggle-is-favorite="emit('toggleIsFavorite', item)"
    />
  </div>
</template>
