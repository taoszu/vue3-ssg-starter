<script setup lang="ts">
import api from '~/apis'
import type ExampleInterface from '~/types/example.interface'

const response = ref<ExampleInterface[]>([])
const postTotal = ref<Number>(20)
const posts = computed(() => response.value.filter((post: ExampleInterface, i) => i <= postTotal.value))

const getData = async() => {
  const { data } = await api.get<ExampleInterface[]>('posts')

  response.value = data
}

if (response.value.length === 0) getData()

onServerPrefetch(async() => {
  await getData()
})

useHead({
  title: 'Example',
})

</script>

<template>
  <div class="container mx-auto">
    <h1 class="text-2xl lg:text-3xl text-center capitalize font-black bg-gradient-to-r bg-clip-text text-transparent from-blue-500 to-blue-700 mb-5">
      fake json response
    </h1>

    <p>
      Showing {{ postTotal }} Posts
    </p>

    <example-dropdown class="mb-5 mt-3" @change:post-total="postTotal = $event" />

    <div class="flex flex-wrap gap-2">
      <router-link v-for="(item, i) in posts" :key="i" :to="`/example/${item.id}`" as="div" class="bg-blue-100 dark:bg-blue-500 p-2 rounded-md transition hover:bg-blue-200 hover:text-blue-600 dark:hover:bg-blue-400 dark:hover:text-gray-200">
        {{ item.title }}
      </router-link>
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: example
</route>
