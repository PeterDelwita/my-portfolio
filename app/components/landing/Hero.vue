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
  <UPageHero
    class="min-h-0! h-auto!"
    :ui="{
      root: 'py-0 sm:py-0 min-h-0',
      wrapper: 'min-h-0 !h-auto', // override internal wrapper height
      container: 'py-0',
      headline: 'flex items-center justify-center',
      title: 'text-shadow-md max-w-lg mx-auto',
      description: 'max-w-2xl mx-auto',
      links: 'flex-col justify-center items-center'
    }"
  >
    <template #title>
      <Motion
        :key="props.page.title"
        :initial="{ scale: 1.1, opacity: 0, filter: 'blur(20px)' }"
        :animate="{ scale: 1, opacity: 1, filter: 'blur(0px)' }"
        :transition="{ duration: 0.6, delay: 0.1 }"
      >
        {{ props.page.title }}
      </Motion>
    </template>

    <template #description>
      <Motion
        :key="props.page.description"
        :initial="{ scale: 1.1, opacity: 0, filter: 'blur(20px)' }"
        :animate="{ scale: 1, opacity: 1, filter: 'blur(0px)' }"
        :transition="{ duration: 0.6, delay: 0.3 }"
      >
        {{ props.page.description }}
      </Motion>
    </template>

    <template #links>
      <Motion
        :key="JSON.stringify(props.page.hero?.links)"
        :initial="{ scale: 1.1, opacity: 0, filter: 'blur(20px)' }"
        :animate="{ scale: 1, opacity: 1, filter: 'blur(0px)' }"
        :transition="{ duration: 0.6, delay: 0.5 }"
      >
        <div
          v-if="props.page.hero?.links?.length"
          class="flex items-center gap-2"
        >
          <UButton v-bind="props.page.hero.links[0]" />
        </div>
      </Motion>
    </template>
  </UPageHero>
</template>
