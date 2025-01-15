<template>
  <select v-model="model" name="" id="">
    <option v-for="option in options" :value="option.value" :key="option.value">
      {{ option.label }}
    </option>
  </select>
</template>

<script setup>
const props = defineProps({
  options: {
    type: Array,
    required: true,
  },
})

const model = defineModel()
</script>
