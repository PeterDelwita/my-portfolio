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
      container: 'p-0!',
      title: 'text-left text-xl sm:text-xl lg:text-2xl font-medium',
      description: 'text-left mt-3 whitespace-pre-line text-sm sm:text-md lg:text-sm text-muted mb-2'
    }"
  />
</template>
