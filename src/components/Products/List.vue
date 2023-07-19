<script setup>

  import { ref, computed } from 'vue'

  import ProductsListPrice from './List/Price.vue'
  import ProductsListOrderBy from './List/OrderBy.vue'
  import ProductsListItem from './List/Item.vue'

  const props = defineProps({ products: Object })

  const price = ref(1000)
  const orderBy = ref('')

  const filteredProducts = computed(() => {
    const field = orderBy.value

    return props.products
      .filter(p => p.price <= price.value)
      .sort((a, b) => a[field] - b[field])
  })

</script>

<template>

  <div class="product">
    <div class="row border-top border-bottom py-2 mb-3">
      <div class="col-sm-6">
        <ProductsListPrice v-model:price="price" />
      </div>
      <div class="col-sm-6">
        <ProductsListOrderBy v-model:orderBy="orderBy" />
      </div>
    </div>

    <ProductsListItem
      v-for="product in filteredProducts"
      :key="product.id"
      :product="product"
    />
  </div>

</template>