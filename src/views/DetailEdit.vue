<script setup lang="ts">
import InputForm from '@/components/InputForm.vue'
import { useRoute, useRouter } from 'vue-router'
import SUPABASE from '@/supabaseClient'
import { ref } from 'vue'

const route = useRoute()
const router = useRouter()
const slug = ref<any>(route.params.id)
const post = ref<any>()

const setTitle = ref<string>('')
const setSynopsis = ref<string>('')
const setSlug = ref<string>('')
const setAuthor = ref<string>('')
const setText = ref<string>('')

async function getPost() {
  try {
    const { data: resultPost, error: errorPost } = await SUPABASE.from('posts')
      .select('id, title, text, synopsis, slug, author, created_at')
      .eq('slug', slug.value)
      .single()

    if (errorPost !== null) {
      throw errorPost
    }
    setText.value = resultPost.text
    post.value = resultPost
  } catch (e) {
    console.log(e)
  }
}

async function onUpdateData() {
  try {
    const { error: errorUpdate } = await SUPABASE.from('posts')
      .update({
        title: setTitle.value != '' ? setTitle.value : post.value.title,
        synopsis: setSynopsis.value != '' ? setSynopsis.value : post.value.synopsis,
        slug: setSlug.value != '' ? setSlug.value : post.value.slug,
        author: setAuthor.value != '' ? setAuthor.value : post.value.author,
        text: setText.value
      })
      .eq('id', post.value.id)

    if (errorUpdate !== null) {
      throw errorUpdate
    }

    // REDIRECT
    router.push({ name: 'home' })
  } catch (e) {
    console.log(e)
  }
}

getPost()
</script>

<template>
  <main class="py-10">
    <form id="form__modal" name="form__modal" class="py-5 w-full max-w-[800px] mx-auto" v-if="post">
      <InputForm
        @update:modelValue="(newValue: string) => (setTitle = newValue)"
        class="mb-5"
        title="Title"
        id="title"
        type="text"
        :value="post.title"
        name="title"
      />
      <InputForm
        @update:modelValue="(newValue: string) => (setSynopsis = newValue)"
        class="mb-5"
        title="Synopsis"
        id="synopsis"
        type="text"
        :value="post.synopsis"
        name="synopsis"
      />
      <InputForm
        @update:modelValue="(newValue: string) => (setSlug = newValue)"
        class="mb-5"
        title="Slug"
        id="slug"
        type="text"
        :value="post.slug"
        name="slug"
      />
      <InputForm
        @update:modelValue="(newValue: string) => (setAuthor = newValue)"
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
          v-model="setText"
          type="text"
          name="text"
          id="text"
          class="bg-gray-100 w-full focus:outline-none py-2 px-4 rounded"
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
