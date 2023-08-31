<script setup lang="ts">
import HeaderHome from '@/components/HeaderHome.vue'
import CardArticle from '@/components/CardArticle.vue'

import SUPABASE from '@/supabaseClient'
import { ref } from 'vue'

const allPosts = ref<any>()
const posts = ref<any>()

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

getPosts()
</script>

<template>
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
</template>
