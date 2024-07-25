<script setup lang="ts">
import { defineProps, computed } from 'vue'

const props = defineProps<{
  selectedProducts: Array<{ name: string; count: number; price: number; image: string }>
}>()

const totalAmount = computed(() => {
  return props.selectedProducts.reduce((total, product) => total + product.count * product.price, 0)
})
</script>
<template>
  <div class="bg-white p-4 rounded-lg shadow-sm">
    <div class="">
      <h2 class="text-lg font-bold text-red-600">Your Cart (<span>0</span>)</h2>

      <div v-if="selectedProducts.length > 0">
        <ul>
          <li
            v-for="product in selectedProducts"
            :key="product.name"
            class="mb-2 flex items-center"
          >
            <img :src="product.image" alt="product.name" class="w-12 h-12 mr-4" />
            <div>
              {{ product.name }} - {{ product.count }} x ${{ product.price }} = ${{
                product.count * product.price
              }}
            </div>
          </li>
        </ul>
        <div class="font-bold text-xl mt-5">Total: ${{ totalAmount }}</div>
      </div>

      <div v-else>
        <div class="flex justify-center py-8">
          <img src="@/assets/images/illustration-empty-cart.svg" />
        </div>
        <p class="py-2 text-center">Your added items will appear here</p>
      </div>
    </div>
  </div>
</template>
