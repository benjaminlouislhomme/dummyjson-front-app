<script setup>
  import { ref } from 'vue'

  const props = defineProps({ category: String })
  const emit = defineEmits(['update:category'])

  const endPoint = 'https://dummyjson.com/products/'
  const categoriesResponse = await fetch(endPoint + 'categories')
  const categories = ref((await categoriesResponse.json()))

  const category = ref(props.category)

</script>
<template>

  <div>
    <ul>
      <li
        v-for="cat in categories"
        :class="{ selected: cat == props.category }"
      >
        <a href="#" @click="category = cat, $emit('update:category', cat)">
          {{ cat }}
        </a>
      </li>
    </ul>
  </div>

</template>

<style scoped>

  .selected {
    background: yellow;
    color: red;
  }

</style>