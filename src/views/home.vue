<template>
  <div>
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-6">
      <!-- Loop over the products array -->
      <div v-for="product in products" :key="product.id">
        <product-card :imageSrc="product.imageSrc" :name="product.name" :price="product.price"
          :description="product.description" :inStock="product.inStock" v-model:liked="product.liked"
          @add-to-cart="handleAddToCart" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import productCard from '../components/product-card.vue';

const products = ref([
  {
    id: 1,
    imageSrc: "https://justfields.com/storage/projects/7M5rV059/product-card-img.jpg",
    name: "Apple Watch Series 7 GPS",
    price: "29.99",
    description: "Lorem ipsum dolor amet Lorem ipsum dolor",
    inStock: "true",
    liked: false
  },
  {
    id: 2,
    imageSrc: "https://justfields.com/storage/projects/7M5rV059/product-card-img.jpg",
    name: "Samsung Galaxy Watch 4",
    price: "39.99",
    description: "A sleek smartwatch with advanced features.",
    inStock: "true",
    liked: false
  },
  {
    id: 3,
    imageSrc: "https://justfields.com/storage/projects/7M5rV059/product-card-img.jpg",
    name: "Fitbit Versa 3",
    price: "49.99",
    description: "A fitness tracker that helps you monitor your.",
    inStock: "true",
    liked: false
  }
]);

const handleAddToCart = (newCartCount) => {
  console.log('Item added to cart, new count:', newCartCount);
};
</script>