<script setup>

  import { ref } from 'vue'

  import ProductsForm from './Products/Form.vue'
  import ProductsList from './Products/List.vue'

  const productsResponse = await fetch('https://dummyjson.com/products')
  const products = ref((await productsResponse.json()).products)

</script>
<template>

  <h2 class="text-center">Products</h2>

  <div class="row">
    <div class="col-sm-3">
      <ProductsForm
        :products="products"
        @update-products="(updatedProducts) => products = updatedProducts"
      />
    </div>

    <div class="col-sm-9">
      <ProductsList :products="products" />
    </div>
  </div>

</template>
