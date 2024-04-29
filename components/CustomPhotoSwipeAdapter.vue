<script setup lang="ts">
import { computed } from 'vue'
import type { PhotoRendererMetadata } from 'vue-photo-album'

const props = defineProps<PhotoRendererMetadata>()
const largePhoto = computed(() => props.photo.srcSet?.[0] ?? props.photo)
</script>

<template>
  <a
    data-fancybox="gallery"
    :href="largePhoto.src"
  >
    <slot />
  </a>
</template>
