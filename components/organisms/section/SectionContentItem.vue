<template>
  <div class="flex flex-col items-center">
    <slot name="image">
      <img class="mb-4" :src="image" :alt="`A ${title} image`" />
    </slot>
    <h3 class="mb-1 text-center text-body-lg lg:text-body-xl font-bold">{{ title }}</h3>
    <p class="text-center text-sm lg:text-base d-secondary-text mb-4">{{ description }}</p>
    <MarketingLink v-if="to" color="primary-green" :name="linkName" icon="IconChevronRight" :to="localePath(to)" />
  </div>
</template>

<script>
import { defineComponent } from '@nuxtjs/composition-api'

export default defineComponent({
  props: {
    image: {
      type: String,
      default: ''
    },
    title: {
      type: String,
      default: ''
    },
    description: {
      type: String,
      default: ''
    },
    to: {
      type: String,
      default: ''
    },
    linkName: {
      type: String,
      default: 'Learn More'
    }
  }
})
</script>
