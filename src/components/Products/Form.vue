<script setup>
  import { ref, watch } from 'vue'

  import ProductsFormSearch from './Form/Search.vue'
  import ProductsFormCategories from './Form/Categories.vue'

  const props = defineProps({ products: Object })
  const emit = defineEmits([ 'updateProducts' ])

  const products = ref(props.products)
  const updatedProducts = ref([])
  const q = ref('')
  const category = ref('')

  const fetchProducts = async (url) => {
    const response = await fetch(`https://dummyjson.com/products${ url }`)
    updatedProducts.value = (await response.json()).products
    emit('updateProducts', updatedProducts.value)
  }

  const fetchProductsBySearch = async (q) => {
    fetchProducts(`/search?q=${ q }`)
    category.value = null
  }

  const fetchProductsByCategory = async (category) => {
    fetchProducts(`/category/${ category }`)
    q.value = null
  }

</script>

<template>

  <div class="bg-light rounded p-2">
    <ProductsFormSearch
      v-model:q="q"
      @update:q="fetchProductsBySearch"
    />

    <br>

    <ProductsFormCategories
      v-model:category="category"
      @update:category="fetchProductsByCategory"
    />
  </div>
  
</template>