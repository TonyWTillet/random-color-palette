<script setup>

const props = defineProps({
  message: {
    type: String,
    required: false,
    default: 'Color copied to clipboard!'
  },
  active: {
    type: Boolean,
    required: false,
    default: false
  },
  type: {
    type: String,
    values: ['success', 'error', 'warning'],
    required: false,
    default: true
  }
})
</script>

<template>
  <!-- Notification - color copied -->
  <div v-if="active"
     class="fixed bottom-4 right-4 bg-green-500 text-white px-4 py-2 rounded-lg shadow-lg transition-opacity duration-300"  
     :class="`bg-${type}-500`">
    {{ message }}
  </div>
</template>