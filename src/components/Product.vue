<script setup>

  import { ref } from 'vue'
  import { useRoute } from 'vue-router'

  const route = useRoute()
  
  const { params } = route
  const { id } = params

  const url = `https://dummyjson.com/product/${ id }`
  const response = await fetch(url)
  const product = await response.json()

  const {
    title,
    description,
    price,
    discountPercentage,
    rating,
    stock,
    brand,
    category,
    thumbnail,
    images
  } = product

  const featuredImg = ref(thumbnail)

</script>
<template>

  <div id="product" class="border rounded pt-4">

    <div id="header">

      <h1 class="text-center">{{ title }}</h1>
      <h2 class="text-center">
        ${{ price }} <small>(- {{ discountPercentage }} %)</small>
      </h2>
    </div>

    <div class="row p-4">
      <div class="col-sm-8">
        <img :src="featuredImg" class="w-100">

        <hr>

        <ul class="d-flex">
          <li v-for="img in images" @click="featuredImg = img">
            <img :src="img" class="w-75">
          </li>
        </ul>

      </div>
      <div class="col-sm-4">
        <p>Stock: {{ stock }}</p>
        <p>Brand: {{ brand }}</p>
        <p>Category: {{ category }}</p>
      </div>
    </div>

    <p class="fst-italic">
      {{ description }}
    </p>
  
  </div>

</template>
<style scoped>

  ul li {
    list-style: none;
    cursor: pointer;
  }

  ul li img:hover {
    filter: brightness(.5) grayscale(.5);
    transition: .5s;
  }

</style>