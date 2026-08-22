<template>
  <input
    :value="modelValue"
    class="search"
    type="search"
    placeholder="Search by name"
    @input="handleInput"
  >
</template>

<script setup>
defineProps({
  modelValue: {
    type: String,
    default: '',
  },
})

const emit = defineEmits(['update:modelValue'])

function handleInput(event) {
  emit('update:modelValue', event.target.value)
}
</script>

<style scoped>
.search {
  width: 100%;
  margin-bottom: 20px;
  padding: 8px;
  box-sizing: border-box;
}
</style>
