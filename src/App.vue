<script setup>
import { computed, onMounted, ref, watch } from 'vue'
import ProductCard from './components/ProductCard.vue'
import CartItem from './components/CartItem.vue'

const products = ref([])
const loading = ref(true)
const searchQuery = ref('')
const selectedCategory = ref('Усі категорії')
const isCartOpen = ref(false)
const cart = ref([])

const categories = computed(() => [
  'Усі категорії',
  ...new Set(products.value.map(product => product.category))
])

const filteredProducts = computed(() => {
  const query = searchQuery.value.trim().toLowerCase()

  return products.value.filter(product => {
    const matchesCategory =
      selectedCategory.value === 'Усі категорії' ||
      product.category === selectedCategory.value
    const matchesSearch = product.name.toLowerCase().includes(query)
    return matchesCategory && matchesSearch
  })
})

const cartCount = computed(() =>
  cart.value.reduce((total, item) => total + item.quantity, 0)
)

const cartTotal = computed(() =>
  cart.value.reduce((total, item) => total + item.price * item.quantity, 0)
)

const formatPrice = value => `${value.toLocaleString('uk-UA')} ₴`

onMounted(async () => {
  try {
    const response = await fetch(`${import.meta.env.BASE_URL}products.json`)
    if (!response.ok) throw new Error('Не вдалося завантажити каталог')
    products.value = await response.json()

    const savedCart = localStorage.getItem('techstore-cart')
    if (savedCart) cart.value = JSON.parse(savedCart)
  } catch (error) {
    console.error(error)
  } finally {
    loading.value = false
  }
})

watch(cart, newCart => {
  localStorage.setItem('techstore-cart', JSON.stringify(newCart))
}, { deep: true })

function addToCart(product) {
  const existingItem = cart.value.find(item => item.id === product.id)

  if (existingItem) {
    existingItem.quantity += 1
  } else {
    cart.value.push({ ...product, quantity: 1 })
  }
  isCartOpen.value = true
}

function increaseQuantity(id) {
  const item = cart.value.find(item => item.id === id)
  if (item) item.quantity += 1
}

function decreaseQuantity(id) {
  const item = cart.value.find(item => item.id === id)
  if (!item) return

  if (item.quantity > 1) {
    item.quantity -= 1
  } else {
    removeFromCart(id)
  }
}

function removeFromCart(id) {
  cart.value = cart.value.filter(item => item.id !== id)
}

function clearCart() {
  cart.value = []
}

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: 'smooth' })
}

function showDemoMessage() {
  window.alert('Дякуємо! Це демонстраційний навчальний магазин.')
}
</script>

<template>
  <div class="site-shell">
    <header class="site-header">
      <a class="brand" href="#" @click.prevent="scrollToTop">
        <span class="brand-mark">T</span>
        <span>TECH<span class="brand-accent">STORE</span></span>
      </a>

      <nav class="main-nav">
        <a href="#catalog">Каталог</a>
        <a href="#about">Про магазин</a>
      </nav>

      <button class="header-cart" @click="isCartOpen = true">
        <span class="cart-icon">🛒</span>
        <span>Кошик</span>
        <span class="cart-count">{{ cartCount }}</span>
      </button>
    </header>

    <main>
      <section class="hero">
        <div class="hero-content">
          <span class="eyebrow"><span class="eyebrow-dot"></span> ТЕХНІКА ДЛЯ ТВОЇХ ІДЕЙ</span>
          <h1>Технології, які<br /><span>рухають вперед.</span></h1>
          <p>Обирай комп'ютерну техніку для роботи, навчання та яскравих перемог.</p>
          <a class="hero-button" href="#catalog">Перейти до каталогу <span>→</span></a>
        </div>
        <div class="hero-art" aria-hidden="true">
          <div class="hero-orbit orbit-one"></div>
          <div class="hero-orbit orbit-two"></div>
          <div class="hero-chip">TECH<br />STORE</div>
          <div class="hero-spark spark-one">✦</div>
          <div class="hero-spark spark-two">+</div>
          <div class="hero-caption">UPGRADE<br />YOUR SETUP</div>
        </div>
        <div class="hero-bottom">
          <span>01 / 04</span>
          <span class="hero-line"></span>
          <span>ПІДБЕРИ СВОЮ ТЕХНІКУ</span>
        </div>
      </section>

      <section id="catalog" class="catalog-section">
        <div class="section-heading">
          <div>
            <span class="section-kicker">НАШ АСОРТИМЕНТ</span>
            <h2>Каталог <span>товарів</span></h2>
          </div>
          <p>Знайди техніку, яка підходить саме тобі.</p>
        </div>

        <div class="catalog-toolbar">
          <div class="category-list">
            <button
              v-for="category in categories"
              :key="category"
              :class="['category-button', { active: selectedCategory === category }]"
              @click="selectedCategory = category"
            >
              {{ category }}
            </button>
          </div>
          <label class="search-box">
            <span>⌕</span>
            <input v-model="searchQuery" type="search" placeholder="Пошук товару..." />
          </label>
        </div>

        <div v-if="loading" class="state-message">Завантажуємо каталог...</div>
        <div v-else-if="filteredProducts.length" class="product-grid">
          <ProductCard
            v-for="product in filteredProducts"
            :key="product.id"
            :product="product"
            @add-to-cart="addToCart"
          />
        </div>
        <div v-else class="state-message empty-results">
          <span class="empty-icon">⌕</span>
          <h3>Нічого не знайдено</h3>
          <p>Спробуй змінити пошуковий запит або категорію.</p>
          <button class="reset-button" @click="searchQuery = ''; selectedCategory = 'Усі категорії'">
            Скинути фільтри
          </button>
        </div>
      </section>

      <section id="about" class="about-strip">
        <div><span class="about-number">01</span><strong>Якісна техніка</strong><small>Перевірені товари</small></div>
        <div><span class="about-number">02</span><strong>Зручний вибір</strong><small>Каталог за категоріями</small></div>
        <div><span class="about-number">03</span><strong>Простий кошик</strong><small>Підрахунок у реальному часі</small></div>
      </section>
    </main>

    <footer class="site-footer">
      <a class="brand footer-brand" href="#"><span class="brand-mark">T</span><span>TECH<span class="brand-accent">STORE</span></span></a>
      <span>© 2026 TechStore. Навчальний проєкт на Vue.js 3.</span>
      <a href="#catalog">До каталогу ↑</a>
    </footer>

    <div v-if="isCartOpen" class="modal-backdrop" @click.self="isCartOpen = false">
      <aside class="cart-panel">
        <div class="cart-header">
          <div>
            <span class="section-kicker">ВАШЕ ЗАМОВЛЕННЯ</span>
            <h2>Кошик <span>({{ cartCount }})</span></h2>
          </div>
          <button class="close-cart" aria-label="Закрити кошик" @click="isCartOpen = false">×</button>
        </div>

        <div v-if="cart.length" class="cart-content">
          <div class="cart-items">
            <CartItem
              v-for="item in cart"
              :key="item.id"
              :item="item"
              @increase="increaseQuantity"
              @decrease="decreaseQuantity"
              @remove="removeFromCart"
            />
          </div>
          <div class="cart-summary">
            <div class="summary-line"><span>Кількість товарів</span><strong>{{ cartCount }}</strong></div>
            <div class="summary-total"><span>Разом до сплати</span><strong>{{ formatPrice(cartTotal) }}</strong></div>
            <button class="checkout-button" @click="showDemoMessage">
              Оформити замовлення <span>→</span>
            </button>
            <button class="clear-cart" @click="clearCart">Очистити кошик</button>
          </div>
        </div>

        <div v-else class="empty-cart">
          <div class="empty-cart-icon">🛒</div>
          <h3>Кошик поки порожній</h3>
          <p>Додай товари з каталогу — тут з'явиться твоє замовлення.</p>
          <button class="checkout-button" @click="isCartOpen = false">Перейти до покупок</button>
        </div>
      </aside>
    </div>
  </div>
</template>