<script setup lang="ts">
const props = defineProps({
  title: {
    type: String,
    default: '',
  },
})
</script>

<template>
  <TheCallout :title="props.title" type="danger">
    <slot />
  </TheCallout>
</template>
