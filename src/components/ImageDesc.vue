<script setup>
defineProps({
  name: String,
  descStyle: String,
})
</script>

<template>
  <div :style="descStyle">
    Author: {{ name }}
  </div>
</template>

<style scoped>
div {
  position: fixed;
  pointer-events: none;
  z-index: 999;
  white-space: nowrap;
  background-color: var(--vt-c-black-trans);
  padding: 0.5rem;
  border-radius: 0.5rem;
  font-size: 0.85rem;
}
</style>