<script setup lang="ts">
import { defineProps, reactive, onMounted, watch } from 'vue'
import products from '../data.json'
import SelectedProducts from './AddCart.vue'

// Function to get the appropriate image URL based on type
function getImage(product: any, type: 'thumbnail' | 'mobile' | 'tablet' | 'desktop'): string {
  return product.image[type]
}

// Reactive object to keep track of counts for each product
const productCounts = reactive<{ [key: string]: number }>({})
const selectedProducts = reactive<
  Array<{ name: string; count: number; price: number; image: string }>
>([])

products.forEach((product) => {
  productCounts[product.name] = 0
})

// Load selected products from local storage on mount
onMounted(() => {
  const storedProducts = localStorage.getItem('selectedProducts')
  if (storedProducts) {
    const parsedProducts = JSON.parse(storedProducts)
    parsedProducts.forEach(
      (product: { name: string; count: number; price: number; image: string }) => {
        productCounts[product.name] = product.count
        selectedProducts.push(product)
      }
    )
  }
})

// Watch selectedProducts and save to local storage whenever it changes
watch(
  () => selectedProducts,
  (newProducts) => {
    localStorage.setItem('selectedProducts', JSON.stringify(newProducts))
  },
  { deep: true }
)

function increment(productName: string, productPrice: number, productImage: string) {
  productCounts[productName]++
  updateSelectedProducts(productName, productPrice, productImage)
}

function decrement(productName: string, productPrice: number, productImage: string) {
  if (productCounts[productName] > 0) {
    productCounts[productName]--
    updateSelectedProducts(productName, productPrice, productImage)
  }
}

function updateSelectedProducts(productName: string, productPrice: number, productImage: string) {
  const existingProduct = selectedProducts.find((product) => product.name === productName)
  if (existingProduct) {
    existingProduct.count = productCounts[productName]
    if (existingProduct.count === 0) {
      const index = selectedProducts.indexOf(existingProduct)
      selectedProducts.splice(index, 1)
    }
  } else if (productCounts[productName] > 0) {
    selectedProducts.push({
      name: productName,
      count: productCounts[productName],
      price: productPrice,
      image: productImage
    })
  }
}
</script>

<template>
  <div class="flex">
    <div class="w-3/4">
      <h1 class="text-5xl mb-5 font-bold">Desserts</h1>
      <div class="flex justify-between flex-wrap flex-row">
        <div v-for="product in products" :key="product.name" class="w-1/3 p-2 group transition-all">
          <div class="w-full">
            <div class="group-hover:border-red-500 border-2 ease-in-out rounded-xl">
              <img
                class="w-full rounded-xl"
                :src="getImage(product, 'desktop')"
                :alt="product.name"
              />
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
                  <img
                    @click="decrement(product.name, product.price, getImage(product, 'desktop'))"
                    src="@/assets/images/icon-decrement-quantity.svg"
                    alt="decrement-quantity"
                  />
                </div>
                <div class="text-white">{{ productCounts[product.name] }}</div>
                <div>
                  <img
                    @click="increment(product.name, product.price, getImage(product, 'desktop'))"
                    src="@/assets/images/icon-increment-quantity.svg"
                    alt="increment-quantity"
                  />
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
    </div>

    <div class="w-1/4">
      <SelectedProducts :selectedProducts="selectedProducts" />
    </div>

    <!-- Use the SelectedProducts component -->
  </div>
</template>
