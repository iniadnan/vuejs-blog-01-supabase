<script setup lang="ts">
const emit = defineEmits<{
  toggleModal: [value: string]
}>()
</script>

<template>
  <nav class="w-full border-b-2 py-3 bg-gray-50">
    <div class="container mx-auto px-5 flex items-center justify-between">
      <RouterLink :to="{ name: 'home' }" class="font-semibold text-xl md:text-3xl text-gray-700"
        >Home</RouterLink
      >
      <button
        type="button"
        @click="$emit('toggleModal')"
        class="text-base text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium py-2 px-5 rounded-lg"
      >
        New Post
      </button>
    </div>
  </nav>
</template>

<style scoped></style>
