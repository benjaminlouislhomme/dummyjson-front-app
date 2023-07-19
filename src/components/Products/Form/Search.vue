<script setup>

  const props = defineProps({ q: String })
  const emit = defineEmits(['update:q', 'update:products'])

</script>
<template>

  <div>
    <input
      type="text"
      :value="q"
      placeholder="Search"
      @input="$emit('update:q', $event.target.value)"
    />
  </div>

</template>