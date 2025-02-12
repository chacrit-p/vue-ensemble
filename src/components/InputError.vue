<script setup>
defineProps({
  message: {
    required: true,
  },
});
</script>

<template>
  <span v-if="message" class="fieldset-label text-red-500 font-semibold">
    {{ message }}
  </span>
</template>
