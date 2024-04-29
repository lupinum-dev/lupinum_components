<script setup lang="ts">
const props = defineProps({
  size: {
    type: String,
    default: 'md',
  },
})

const sizes = {
  'sm': {
    imgCols: 'col-span-2',
    otherCols: 'col-span-8',
  },
  'md': {
    imgCols: 'col-span-3',
    otherCols: 'col-span-7',
  },
  'lg': {
    imgCols: 'col-span-4',
    otherCols: 'col-span-6',
  },
  'xl': {
    imgCols: 'col-span-5',
    otherCols: 'col-span-5',
  },
  '2xl': {
    imgCols: 'col-span-6',
    otherCols: 'col-span-4',
  },
  '3xl': {
    imgCols: 'col-span-7',
    otherCols: 'col-span-3',
  },
}

// Access slots in a reactive way
const slots = useSlots()

// Memoize the processed slots
const processedSlots = computed(() => {
  const slotNodes = slots.default ? slots.default() : []
  return {
    imgSlot: slotNodes[0],
    remainderSlot: slotNodes.slice(1),
  }
})

function renderContent() {
  const sizeConfig = sizes[props.size] || sizes.sm
  return h('div', { class: `lg:grid grid-cols-10 lg:gap-10 lg:place-items-center flex flex-col` }, [
    h('div', { class: `${sizeConfig.imgCols} not-prose place-content-center mx-auto` }, processedSlots.value.imgSlot),
    h('div', { class: `${sizeConfig.otherCols}` }, processedSlots.value.remainderSlot),
  ])
}
</script>

<template>
  <component :is="renderContent" />
</template>
