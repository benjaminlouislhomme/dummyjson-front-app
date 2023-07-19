<script setup>

  const props = defineProps(['price'])
  const emit = defineEmits(['update:price'])

</script>

<template>

  <div class="row">
    <div class="col-sm-4">
      <label>Max price : ${{ price }}</label>
    </div>
    <div class="col-sm-8">
      <input
        type="range"
        :value="price"
        min="10"
        max="2000"
        step="10"
        class="form-range"
        @input="$emit('update:price', $event.target.value)"
      >
    </div>
  </div>
  
</template>