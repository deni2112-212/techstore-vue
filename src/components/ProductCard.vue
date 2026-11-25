<script setup>
defineProps({
  product: { type: Object, required: true }
})

defineEmits(['add-to-cart'])
</script>

<template>
  <article class="product-card">
    <div class="product-image-wrap">
      <img
        class="product-image"
        :src="product.image"
        :alt="product.name"
        loading="lazy"
        @error="$event.target.src = 'https://placehold.co/600x420/172338/ffffff?text=TechStore'"
      />
      <span v-if="product.tag" class="product-tag">{{ product.tag }}</span>
    </div>

    <div class="product-info">
      <span class="product-category">{{ product.category }}</span>
      <h3>{{ product.name }}</h3>
      <p class="product-description">{{ product.description }}</p>
      <div class="product-bottom">
        <strong class="product-price">{{ product.price.toLocaleString('uk-UA') }} ₴</strong>
        <button class="add-button" @click="$emit('add-to-cart', product)">
          <span>+</span> До кошика
        </button>
      </div>
    </div>
  </article>
</template>