<script setup>
defineProps({
  item: { type: Object, required: true }
})

defineEmits(['increase', 'decrease', 'remove'])
</script>

<template>
  <div class="cart-item">
    <img :src="item.image" :alt="item.name" class="cart-item-image" />
    <div class="cart-item-info">
      <h4>{{ item.name }}</h4>
      <span class="cart-item-price">{{ item.price.toLocaleString('uk-UA') }} ₴ / шт.</span>
      <div class="quantity-row">
        <div class="quantity-control">
          <button aria-label="Зменшити кількість" @click="$emit('decrease', item.id)">−</button>
          <span>{{ item.quantity }}</span>
          <button aria-label="Збільшити кількість" @click="$emit('increase', item.id)">+</button>
        </div>
        <strong>{{ (item.price * item.quantity).toLocaleString('uk-UA') }} ₴</strong>
      </div>
    </div>
    <button class="remove-button" aria-label="Видалити товар" @click="$emit('remove', item.id)">×</button>
  </div>
</template>