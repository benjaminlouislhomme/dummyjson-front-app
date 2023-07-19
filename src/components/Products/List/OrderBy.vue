<script setup>
	
	const props = defineProps(['orderBy'])
  const emit = defineEmits(['update:orderBy'])

</script>

<template>

  <div class="row">
    <div class="col-sm-3">
      <label>Order by :</label>
    </div>

    <div class="col-sm-9">
      <select
        :value="orderBy"
        @change="$emit('update:orderBy', $event.target.value)"
        class="form-select"
      >
        <option disabled value="">-</option>
        <option value="price">Price</option>
        <option value="rating">Rating</option>
      </select>
    </div>
  </div>

</template>