<template>
  <section class="relative pb-20 light:bg-gray-50 dark:bg-secondary-darkest">
    <NuxtContainer class="flex flex-col items-center">
      <div class="flex flex-col items-center w-full col-span-12">
        <div class="mb-2">
          <span class="text-tertiary font-bold text-lg">{{ category }} </span>
        </div>
        <h2 class="mb-2 font-serif font-normal text-display-6 md:text-display-5 2xl:text-display-4">
          <Markdown use="title" unwrap="p" />
        </h2>
        <p class="mb-12 font-normal text-center text-body-base md:text-body-lg 2xl:text-body-xl">
          <Markdown use="description" unwrap="p" />
        </p>
        <ul class="grid grid-cols-1 sm:grid-cols-2 gap-16 sm:gap-8 lg:gap-16 py-8">
          <li v-for="post in posts" :key="post.title" class="flex flex-col self-start">
            <NuxtLink :to="localePath(post.to)">
              <div class="aspect-w-16 aspect-h-8 bg-gray-100 overflow-hidden dark:bg-secondary-darker mb-4 rounded-lg">
                <NuxtImg :src="post.imgUrl" width="864" height="378" :alt="post.title" />
              </div>
            </NuxtLink>
            <span class="text-cloud-light text-body-base lg:text-body-lg font-bold mb-2">{{ post.category }}</span>
            <h3 class="text-body-xl lg:text-body-2xl font-bold mb-2">{{ post.title }}</h3>
            <p class="mb-4 text-body-base lg:text-body-lg">{{ post.description }}</p>
            <MarketingLink color="primary-green" :name="$t('common.read_article')" icon="IconChevronRight" :to="localePath(post.to)" />
          </li>
        </ul>
      </div>
    </NuxtContainer>
  </section>
</template>

<script>
import { defineComponent, ref, useContext, useFetch } from '@nuxtjs/composition-api'
export default defineComponent({
  props: {
    category: {
      type: String,
      default: 'Category'
    }
  },
  setup() {
    const { $docus, i18n } = useContext()
    const posts = ref()
    useFetch(async () => {
      const documents = await $docus.search('/blog/', { deep: true }).where({ 'language': i18n.locale }).sortBy('position', 'asc').limit(2).fetch()

      posts.value = documents
    })
    return {
      posts
    }
  }
})
</script>
