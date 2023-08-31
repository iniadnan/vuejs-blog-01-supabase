<script setup lang="ts">
import HeaderHome from '@/components/HeaderHome.vue'
import CardArticle from '@/components/CardArticle.vue'
import NavTop from '@/components/NavTop.vue'
import ModalForm from '@/components/ModalForm.vue'

import SUPABASE from '@/supabaseClient'
import { ref } from 'vue'

const allPosts = ref<any>()
const posts = ref<any>()
const isModalShow = ref<boolean>(false)

async function getPosts() {
  try {
    const { data: resultPosts, error: postsError } = await SUPABASE.from('posts').select(
      'title, text, synopsis, slug, author, created_at'
    )

    if (postsError !== null) {
      throw postsError
    }
    allPosts.value = resultPosts
    posts.value = resultPosts
  } catch (e) {
    console.log(e)
  }
}

async function onDeletePost(slug: string) {
  try {
    const { error } = await SUPABASE.from('posts').delete().eq('slug', slug)

    if (error !== null) {
      throw error
    }

    allPosts.value = posts.value.filter(
      (post: { title: string; slug: string; author: string; text: string; synopsis: string }) => {
        return post.slug != slug
      }
    )

    posts.value = allPosts.value
  } catch (e) {
    console.log(e)
  }
}

function onSearch(val: string) {
  let stringSearch: string = val

  if (stringSearch.length < 1) {
    return (posts.value = allPosts)
  }

  const filteredPosts = allPosts.value.filter((post: any) => {
    return post.title.toLowerCase().includes(stringSearch.toLowerCase())
  })

  posts.value = filteredPosts
}

function afterInsertData() {
  console.log('asd')
  getPosts()
}

function toggleModal() {
  isModalShow.value = !isModalShow.value
}

getPosts()
</script>

<template>
  <NavTop @toggle-modal="toggleModal" />
  <HeaderHome @key-up-search="onSearch"></HeaderHome>
  <main>
    <div class="container mx-auto px-5 w-full md:w-[900px] lg:w-[1200px]">
      <!-- ARTICLE LIST -->
      <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
        <CardArticle
          v-for="(post, index) in posts"
          :key="index"
          :post="post"
          @delete-post="onDeletePost"
        ></CardArticle>
      </div>
      <!-- PAGINATION -->
      <div></div>
    </div>
  </main>
  <ModalForm
    @toggle-modal="toggleModal"
    @insert-data="afterInsertData"
    :class="isModalShow === true ? 'fixed' : 'hidden'"
  />
</template>
