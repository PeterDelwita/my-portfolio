<script setup lang="ts">
import type { IndexCollectionItem } from '@nuxt/content'

const props = defineProps<{
  page: IndexCollectionItem
}>()

const { refresh } = await useAsyncData('index', () => {
  return queryCollection('index').first()
})

onMounted(() => {
  refresh()
})
</script>

<template>
  <UPageSection
    :key="JSON.stringify(props.page.about)"
    :title="props.page.about?.title"
    :description="props.page.about?.description"
    :ui="{
      root: 'py-0!',
      container: 'p-0!',
      title: 'text-left text-xl sm:text-xl lg:text-2xl font-medium',
      description: 'text-left mt-1 whitespace-pre-line text-md sm:text-md lg:text-md text-muted mb-1'
    }"
  />
</template>
