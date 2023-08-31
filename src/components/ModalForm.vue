<script setup lang="ts">
import InputForm from '@/components/InputForm.vue'
import { ref } from 'vue'

const setTitle = ref<string>('')
const setSynopsis = ref<string>('')
const setSlug = ref<string>('')
const setAuthor = ref<string>('')
const setText = ref<string>('')

function onToggleModal() {}
function onInsertData() {}
</script>

<template>
  <div
    class="top-1/2 left-1/2 transform -translate-y-1/2 -translate-x-1/2 pt-0 pb-3 px-6 w-[500px] bg-gray-50 z-10 shadow"
  >
    <div class="py-3 border-b">
      <h3 class="font-medium text-xl text-gray-500">Add New Post</h3>
    </div>
    <form id="form__modal" name="form__modal" class="py-5 max-h-[400px] overflow-y-auto">
      <InputForm
        @update:modelValue="(newValue: string) => (setTitle = newValue)"
        class="mb-5"
        title="Title"
        id="title"
        type="text"
        :value="setTitle"
        name="title"
      />
      <InputForm
        @update:modelValue="(newValue: string) => (setSynopsis = newValue)"
        class="mb-5"
        title="Synopsis"
        id="synopsis"
        type="text"
        :value="setSynopsis"
        name="synopsis"
      />
      <InputForm
        @update:modelValue="(newValue: string) => (setSlug = newValue)"
        class="mb-5"
        title="Slug"
        id="slug"
        type="text"
        :value="setSlug"
        name="slug"
      />
      <InputForm
        @update:modelValue="(newValue: string) => (setAuthor = newValue)"
        class="mb-5"
        title="Author"
        id="author"
        type="text"
        :value="setAuthor"
        name="author"
      />
      <div class="mb-5">
        <label for="text" class="text-base text-gray-700 inline-block pb-2">Text</label>
        <textarea
          @update:modelValue="(newValue: string) => (setText = newValue)"
          type="text"
          name="text"
          id="text"
          class="bg-gray-100 w-full focus:outline-none py-2 px-4 rounded"
          :value="setText"
          rows="5"
        ></textarea>
      </div>
      <div class="flex items-center justify-end gap-x-6 pr-8">
        <button type="button" class="text-base text-gray-700" @click="$emit('toggleModal')">
          Cancel
        </button>
        <button
          type="button"
          class="text-base text-white py-1.5 px-4 rounded-lg bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium"
          @click="onInsertData()"
        >
          Save
        </button>
      </div>
    </form>
  </div>
</template>
