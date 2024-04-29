<script setup>
const props = defineProps({
  cols: {
    type: Number,
  },
})

const gridCols = {
  1: 'sm:grid-cols-1',
  2: 'sm:grid-cols-2',
  3: 'sm:grid-cols-3',
  4: 'sm:grid-cols-4',
  5: 'sm:grid-cols-5',
  6: 'sm:grid-cols-6',
  7: 'sm:grid-cols-7',
  8: 'sm:grid-cols-8',
  9: 'sm:grid-cols-9',
  10: 'sm:grid-cols-10',
}

const gridColsClass = gridCols[props.cols]

// let const = ref('')
// provide('img-row-data', childData)
</script>

<template>
  <div id="demo" class="img-row-col-width not-prose grid grid-cols-2 gap-2 sm:gap-3 sm:p-2 lg:gap-6" :class="gridColsClass">
    <slot />
  </div>
</template>

<style>
.img-row-col-width p img {
  width: 100%;
}
</style>
