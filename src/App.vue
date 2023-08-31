<script setup lang="ts">
import { ref } from 'vue'
import { RouterView } from 'vue-router'
import NavTop from './components/NavTop.vue'
import ModalForm from '@/components/ModalForm.vue'

const isModalShow = ref<boolean>(false)

function toggleModal() {
  isModalShow.value = !isModalShow.value
}
</script>

<template>
  <NavTop @toggle-modal="toggleModal()" />
  <RouterView />
  <ModalForm @toggle-modal="toggleModal()" :class="isModalShow === true ? 'fixed' : 'hidden'" />
</template>

<style scoped></style>
