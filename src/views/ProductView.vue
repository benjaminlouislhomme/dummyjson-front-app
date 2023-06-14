<script setup>
  import Product from '../components/Product.vue'
</script>

<template>
  <main>
    <Suspense>
      <Product />
    </Suspense>
  </main>
</template>