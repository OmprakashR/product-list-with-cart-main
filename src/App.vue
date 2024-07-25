<script setup lang="ts">
import { RouterLink, RouterView } from 'vue-router'
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <div class="container mx-auto p-5">
    <div class="flex justify-between gap-5">
      <div class="w-3/4">
        <header>
          <div class="wrapper">
            <HelloWorld />
            <nav>
              <RouterLink to="/">Home</RouterLink>
              <RouterLink to="/about">About</RouterLink>
            </nav>
          </div>
        </header>
      </div>
      <RouterView class="w-1/4" />
    </div>
  </div>
</template>
