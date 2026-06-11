<script setup lang="ts">
// Fetch page data
const { data: page } = await useAsyncData('index', () =>
  queryCollection('index').first()
)

if (!page.value) {
  throw createError({
    statusCode: 404,
    statusMessage: 'Page not found',
    fatal: true
  })
}

// Fetch top 3 blog posts
const { data: posts } = await useAsyncData('index-blogs', () =>
  queryCollection('blog').order('date', 'DESC').limit(3).all()
)

useSeoMeta({
  title: page.value?.seo?.title || page.value?.title,
  ogTitle: page.value?.seo?.title || page.value?.title,
  description: page.value?.seo?.description || page.value?.description,
  ogDescription: page.value?.seo?.description || page.value?.description,
  ogImage: 'https://ui.nuxt.com/assets/templates/nuxt/portfolio-light.png'
})
</script>

<template>
  <UPage v-if="page">
    <LandingHero :page="page" />
    <UPageSection :ui="{ container: 'flex' }">
      <LandingAbout :page="page" />
    </UPageSection>
    <LandingBlog
      :page="page"
      :posts="posts"
    />
  </UPage>
</template>
