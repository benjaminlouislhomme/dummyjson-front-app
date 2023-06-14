<script setup>

  import { ref, computed, watch } from 'vue'
  import { useRouter, useRoute } from 'vue-router'

  const router = useRouter()

  const q = ref('')
  const price = ref(500)
  const orderBy = ref('')

  const endPoint = 'https://dummyjson.com/products'

  const productsResponse = await fetch(endPoint)
  const products = ref((await productsResponse.json()).products)

  const categoriesResponse = await fetch(endPoint + '/categories')
  const categories = ref((await categoriesResponse.json()))

  const goToProduct = (id) => {
    router.push({ name: 'product', params: { id: id } })
  }

  const fetchProductsByCategory = async (category) => {
    const url = endPoint + '/category/' + category
    const response = await fetch(url)
    products.value = (await response.json()).products
  }

  const list = computed(() => {
    var list = products.value

    if (price.value) {
      list = list.filter(p => p.price <= price.value)
    }

    if (orderBy.value) {
      const { field, order } = orderBy.value

      list = list.sort((a, b) => {
        if (order == 'ASC') return a[field] - b[field]
        if (order == 'DESC') return b[field] - a[field]
      })
    }

    return list
  })

  watch(q, async () => {
    const { value } = q

    let url = endPoint
    if (value) url += `/search?q=${ value }`

    const response = await fetch(url)
    products.value = (await response.json()).products
  })

</script>
<template>

  <h2 class="text-center">Products</h2>

  <div class="row">
    <div class="col-sm-3 mt-3">
      <form class="">

        <div class="pb-2 mb-2 border-bottom">
          <input v-model="q" placeholder="Search" class="w-100" />
        </div>

        <div class="pb-2 mb-2 border-bottom">
          <label>Max price : ${{ price }}</label>
          <input
            v-model="price"
            type="range"
            min="10"
            max="2000"
            step="10"
            class="form-range"
          >
        </div>

        <div class="pb-2 mb-2 border-bottom">
          <label>Order by : {{ orderBy.field }} {{ orderBy.order }}</label>

          <p class="m-0">
            <b>Price : </b>
            <a href="#" @click="orderBy = { field: 'price', order: 'ASC' }">
              ASC
            </a> - 
            <a href="#" @click="orderBy = { field: 'price', order: 'DESC' }">
              DESC
            </a>
          </p>

          <p class="m-0">
            <b>Rating : </b>
            <a href="#" @click="orderBy = { field: 'rating', order: 'ASC' }">
              ASC
            </a> - 
            <a href="#" @click="orderBy = { field: 'rating', order: 'DESC' }">
              DESC
            </a>
          </p>
          
        </div>

        <div class="pb-2 mb-2 border-bottom">
          <!-- {{ categories }} -->

          <span class="fw-bold">Categories :</span>

          <ul>
            <li v-for="category in categories">
              <a href="#" @click="fetchProductsByCategory(category)">
                {{ category }}
              </a>
            </li>
          </ul>

        </div>

      </form>
    </div>

    <div class="col-sm-9">
      Results : {{ list.length }}

      <div
        v-for="{ id, title, price, rating, description, thumbnail } in list"
        @click="goToProduct(id)" 
        class="product border rounded p-2 mb-2 row"
      >

        <div class="col-sm-2">
          <img :src="thumbnail" class="w-100" />
        </div>

        <div class="col-sm-10">
          <p class="fs-5 mb-0 fw-bold">
            {{ title }}
          </p>

          <p class="fs-6">
            Price: ${{ price }} - Rating: {{ rating }}
          </p>

          <p class="fst-italic">
            {{ description }}
          </p>
            
        </div>
      
      </div>

    </div>

  </div>

</template>

<style scoped>

  .product {
    cursor: pointer;
  }

  .product:hover {
    background: #eee;
  }

  .product a {
    color: red !important;
  }

  .product img:hover {
    filter: brightness(.5) grayscale(.5);
    transition: .5s;
  }

</style>
