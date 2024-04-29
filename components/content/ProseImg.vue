<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import { joinURL, withLeadingSlash, withTrailingSlash } from 'ufo'
import { useRuntimeConfig } from '#imports'
import type { NuxtImg } from '#build/components'

const props = defineProps({
  src: {
    type: String,
    default: '',
  },
  alt: {
    type: String,
    default: '',
  },
  width: {
    type: [String, Number],
    default: undefined,
  },
  height: {
    type: [String, Number],
    default: undefined,
  },
})

const fixedRatios = [
  { name: 'square', ratio: '1:1' },
  { name: 'video', ratio: '16:9' },
  { name: 'photo', ratio: '4:3' },
  { name: 'phone', ratio: '9:16' },
  { name: 'banner', ratio: '4:1' },
]

const refinedSrc = computed(() => {
  const _base = withLeadingSlash(withTrailingSlash(useRuntimeConfig().app.baseURL))
  return props.src.startsWith('/') && !props.src.startsWith('//') && _base !== '/' && !props.src.startsWith(_base)
    ? joinURL(_base, props.src)
    : props.src
})

const altPipe = props.alt.includes('|')
const newWidth = ref()
const newHeight = ref()

if (altPipe) {
  const altSplit = props.alt.split('|')
  const properties = altSplit[1].replace(/\s/g, '')
  const isFixedRatio = fixedRatios.find(ratio => properties.includes(ratio.name))
  const dimensionsPattern = /(\d+)x?(\d+)?/
  const dimensionsMatch = properties.match(dimensionsPattern)

  if (dimensionsMatch) {
    newWidth.value = Number.parseInt(dimensionsMatch[1], 10)
    newHeight.value = dimensionsMatch[2] ? Number.parseInt(dimensionsMatch[2], 10) : undefined
  }

  if (isFixedRatio) {
    const ratioParts = isFixedRatio.ratio.split(':').map(Number)
    const ratioValue = ratioParts[0] / ratioParts[1]

    if (newWidth.value && !newHeight.value) {
      newHeight.value = Math.round(newWidth.value / ratioValue)
    }
    else if (!newWidth.value && newHeight.value) {
      newWidth.value = Math.round(newHeight.value * ratioValue)
    }
    else if (!newWidth.value && !newHeight.value) {
      const defaultWidth = 1350
      newWidth.value = defaultWidth
      newHeight.value = Math.round(newWidth.value / ratioValue)
    }
  }

  const alt = altSplit[0].trim()
  // console.log(`Properties: ${properties}`)
  // console.log(`Alt description: ${alt}`)
  // console.log(`Width: ${newWidth.value}, Height: ${newHeight.value}`)
}

const directus_url = props.src.split('/').pop()
const visibleStatus = ref(false)

onMounted(() => {
  visibleStatus.value = true
})
</script>

<template>
  <ClientOnly v-if="visibleStatus">
    <a
      data-fancybox="gallery" :href="`${refinedSrc}`"
    >
      <NuxtImg
        loading="lazy"
        provider="directus"
        :src="directus_url"
        :alt="alt"
        :width="newWidth"
        :height="newHeight"
        style="border-radius: 22px;"
      /></a>
  </ClientOnly>
  <NuxtImg
    v-else
    loading="lazy"
    provider="directus"
    :src="directus_url"
    :alt="alt"
    :width="newWidth"
    :height="newHeight"
  />
</template>
