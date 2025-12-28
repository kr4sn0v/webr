<template>
  <button
    :class="`mb-4 cursor-pointer rounded-md p-2 px-5 ${
      variant === 'primary'
        ? 'bg-blue-500 text-white hover:bg-blue-700'
        : 'bg-gray-100 text-gray-600 hover:bg-gray-200'
    }`"
  >
    <slot></slot>
  </button>
</template>

<script setup>
defineProps({
  variant: {
    type: String,
    default: 'primary',
  },
})
</script>
