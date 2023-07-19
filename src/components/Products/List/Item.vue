<script setup>

  import { ref } from 'vue'
  import { useRouter, useRoute } from 'vue-router'

  const props = defineProps({ product: Object })
  const product = ref(props.product)

  const { id, title, price, rating, description, thumbnail } = product.value

  const router = useRouter()

  const goToProduct = (id) => {
    router.push({ name: 'product', params: { id: id } })
  }

</script>

<template>

  <div
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