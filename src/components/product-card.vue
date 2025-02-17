<template>
    <div>
        <div class="p-8">
            <div
                class="max-w-sm bg-white rounded-lg shadow-md overflow-hidden hover:shadow-xl transition-shadow duration-300 relative">
                <!-- Image Container -->
                <div class="relative h-48 w-full">
                    <img :src="imageSrc" alt="Product Image" class="object-cover w-full h-full"
                        @load="handleImageLoaded" @error="handleImageError" />
                    <div v-if="isImageLoading"
                        class="absolute inset-0 flex items-center justify-center bg-black bg-opacity-50">
                        <span class="text-gray-600 text-lg">Loading... <i
                                class="fa-solid fa-spinner fa-spin-pulse ml-1"></i></span>
                    </div>
                </div>

                <!-- Product Details -->
                <div class="p-4">
                    <h2 class="text-xl font-bold mb-2">{{ name }}</h2>
                    <p class="text-gray-600 mb-4">{{ description }}</p>
                    <div class="flex items-center justify-between">
                        <span class="text-lg font-semibold text-green-600">{{ formattedPrice }}</span>
                        <p class="text-blue-600 font-semibold">In Stock: <span class="text-gray-600 underline">{{
                            inStock }}</span></p>
                    </div>
                    <div class="flex justify-between mt-3">
                        <!-- Like button -->
                        <button @click="toggleFavorite"
                            class="mt-4 flex items-center focus:outline-none transform transition-transform duration-200"
                            :class="{ 'scale-110': favorite }" :disabled="isFavoriteLoading">
                            <!-- If loading, display a spinner icon -->
                            <template v-if="isFavoriteLoading">
                                <i class="fa-solid fa-spinner fa-spin text-red-500"></i>
                                <span class="ml-2 text-sm"
                                    :class="{ 'text-red-500': favorite, 'text-gray-600': !favorite }">
                                    {{ favorite ? 'Liked' : 'Like' }}
                                </span>
                            </template>
                            <!-- Otherwise, show the heart icon and text -->
                            <template v-else>
                                <i
                                    :class="favorite ? 'fa-solid fa-heart text-red-500' : 'fa-regular fa-heart text-red-500'"></i>
                                <span class="ml-2 text-sm"
                                    :class="{ 'text-red-500': favorite, 'text-gray-600': !favorite }">
                                    {{ favorite ? 'Liked' : 'Like' }}
                                </span>
                            </template>
                        </button>

                        <button @click="handleAddToCart"
                            class="bg-blue-500 hover:bg-blue-600 text-white px-3 py-1 rounded"
                            :disabled="isCartLoading">
                            <span v-if="!isCartLoading">
                                <i class="fa-solid fa-cart-shopping me-1"></i>
                                Add to Cart <span v-if="cartCount > 0" class="ml-1">({{ cartCount }})</span>
                            </span>
                            <span v-else>Adding... <i class="fa-solid fa-spinner fa-spin-pulse ml-1"></i></span>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue';

// Define props with validation
const props = defineProps({
    imageSrc: {
        type: String,
        required: true
    },
    name: {
        type: String,
        required: true
    },
    price: {
        type: [Number, String],
        required: true
    },
    description: {
        type: String,
        default: ''
    },
    inStock: {
        type: String
    }
});

// Define emitted events for add-to-cart and toggle-favorite interactions
const emits = defineEmits(['add-to-cart', 'toggle-favorite']);

const isImageLoading = ref(true);
const cartCount = ref(0);
const favorite = ref(false);
const isCartLoading = ref(false)
const isFavoriteLoading = ref(false);

const formattedPrice = computed(() => {
    const priceNum = parseFloat(props.price);
    return isNaN(priceNum) ? props.price : new Intl.NumberFormat('en-US', { style: 'currency', currency: 'USD' }).format(priceNum);
});

function handleImageLoaded() {
    isImageLoading.value = false;
}

function handleImageError() {
    isImageLoading.value = false;
}

const handleAddToCart = async () => {
    isCartLoading.value = true;
    try {
        await new Promise((resolve) => setTimeout(resolve, 1000));
        cartCount.value++;
        emits('add-to-cart', cartCount.value);
    } catch (error) {
        console.error("Error adding to cart:", error);
    } finally {
        isCartLoading.value = false;
    }
};

const toggleFavorite = async () => {
    isFavoriteLoading.value = true;
    try {
        await new Promise(resolve => setTimeout(resolve, 1000));
        favorite.value = !favorite.value;
        emits('toggle-favorite', favorite.value);
    } catch (error) {
        console.error("Error toggling favorite:", error);
    } finally {
        isFavoriteLoading.value = false;
    }
};
</script>