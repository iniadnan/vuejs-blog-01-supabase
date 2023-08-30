<script setup lang="ts">
import InputForm from '@/components/InputForm.vue'
import { useRoute } from 'vue-router'
import SUPABASE from '@/supabaseClient'
import { ref } from 'vue'

const route = useRoute()
const slug = ref<any>(route.params.id)
const post = ref<any>()

async function getPost() {
  try {
    const { data: resultPost, error: errorPost } = await SUPABASE.from('posts')
      .select('id, title, text, synopsis, slug, author, created_at')
      .eq('slug', slug.value)
      .single()

    if (errorPost !== null) {
      throw errorPost
    }
    post.value = resultPost
  } catch (e) {
    console.log(e)
  }
}

async function onUpdateData() {
  console.log('asd')
}

getPost()
</script>

<template>
  <main class="py-10">
    <form id="form__modal" name="form__modal" class="py-5 w-full max-w-[800px] mx-auto" v-if="post">
      <input type="hidden" name="id" id="id" />
      <InputForm
        class="mb-5"
        title="Title"
        id="title"
        type="text"
        :value="post.title"
        name="title"
      />
      <InputForm
        class="mb-5"
        title="Synopsis"
        id="synopsis"
        type="text"
        :value="post.synopsis"
        name="synopsis"
      />
      <InputForm class="mb-5" title="Slug" id="slug" type="text" :value="post.slug" name="slug" />
      <InputForm
        class="mb-5"
        title="Author"
        id="author"
        type="text"
        :value="post.author"
        name="author"
      />
      <div class="mb-5">
        <label for="text" class="text-base text-gray-700 inline-block pb-2">Text</label>
        <textarea
          type="text"
          name="text"
          id="text"
          class="bg-gray-100 w-full focus:outline-none py-2 px-4 rounded"
          :value="post.text"
          rows="5"
        ></textarea>
      </div>
      <div class="flex items-center justify-end gap-x-6 pr-8">
        <button
          type="button"
          class="text-base text-white py-1.5 px-4 rounded-lg bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium"
          @click="onUpdateData()"
        >
          Update
        </button>
      </div>
    </form>
  </main>
</template>

<style></style>
