<script setup>
import { onMounted, onUnmounted } from 'vue'

const emit = defineEmits(['hide'])

let timer = null

onMounted(() => {
  timer = setTimeout(() => emit('hide'), 5000)
})

onUnmounted(() => {
  clearTimeout(timer)
})
</script>

<template>
  <div class="toast" role="status">Спасибо за отзыв!</div>
</template>

<style scoped>
.toast {
  position: fixed;
  bottom: 24px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 1000;
  padding: 12px 24px;
  font-size: 15px;
  font-family: Arial, Helvetica, sans-serif;
  color: #ffffff;
  background: #2e7d32;
  border-radius: 6px;
}
</style>
