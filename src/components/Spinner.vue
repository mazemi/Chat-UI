<template>
  <div class="grow-spinner" :style="{ width, height }" role="status" :aria-label="label"></div>
</template>

<script setup>
defineProps({
  width: { type: String, default: '2rem' },
  height: { type: String, default: '2rem' },
  label: { type: String, default: 'Loading...' },
})
</script>

<style scoped>
.grow-spinner {
  display: inline-block;
  background-color: #9a9c9e; /* Bootstrap blue */
  border-radius: 50%;
  opacity: 0;
  animation: growFade 1s infinite ease-in-out;
}

@keyframes growFade {
  0% {
    transform: scale(0);
    opacity: 0;
  }
  50% {
    transform: scale(1);
    opacity: 1;
  }
  100% {
    transform: scale(0);
    opacity: 0;
  }
}
</style>
