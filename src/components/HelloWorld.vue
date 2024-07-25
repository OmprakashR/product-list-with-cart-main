<script setup lang="ts">
import { defineProps } from 'vue'
import products from '../data.json'

const product = products

// Function to get the appropriate image URL based on type
function getImage(product: any, type: 'thumbnail' | 'mobile' | 'tablet' | 'desktop'): string {
  return product.image[type]
}
</script>

<template>
  <h1 class="text-5xl mb-5 font-bold">Desserts</h1>
  <div class="flex justify-between flex-wrap flex-row">
    <div v-for="product in products" :key="product.name" class="w-1/3 p-2 group transition-all">
      <div class="w-full">
        <div class="group-hover:border-red-500 border-2 ease-in-out rounded-xl">
          <img class="w-full rounded-xl" :src="getImage(product, 'desktop')" :alt="product.name" />
        </div>
        <div class="flex justify-center -mt-5">
          <div
            class="px-5 w-40 py-3 group-hover:hidden cursor-pointer transition-all rounded-full flex gap-3 bg-white border border-red-900"
          >
            <img src="@/assets/images/icon-add-to-cart.svg" alt="add-to-cart" />
            <span class="capitalize font-semibold">Add to cart</span>
          </div>

          <div
            class="px-5 py-3 w-40 justify-between cursor-pointer group-hover:flex transition-all rounded-full hidden gap-3 bg-red-800 items-center border border-red-900"
          >
            <div>
              <button @click="count++">Count is: {{ count }}</button>
              <img src="@/assets/images/icon-decrement-quantity.svg" alt="decrement-quantity" />
            </div>
            <div class="text-white">{{ count }}</div>
            <div>
              <img src="@/assets/images/icon-increment-quantity.svg" alt="increment-quantity" />
            </div>
          </div>
        </div>

        <div class="py-6">
          <div class="text-red-900">{{ product.category }}</div>
          <div class="text-lg font-slate-800 font-medium">{{ product.name }}</div>
          <div class="font-bold text-red-600">
            <span class="font-bold">$</span>{{ product.price }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
import type { count } from 'console';
