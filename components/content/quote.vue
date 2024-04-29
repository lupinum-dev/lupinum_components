<script setup>
const props = defineProps({
  quote: {
    type: String,
    default: '',
  },
  author: {
    type: String,
    default: '',
  },
})
</script>

<template>
  <div class="prose-code:bg-white dark:prose-code:bg-gray-900 group relative my-5 block rounded-lg border-l-4  border-teal-500 bg-teal-500/10 py-3 pl-4 pr-6  font-normal last:mb-0">
    <div class=" flex items-center justify-between ">
      <UIcon name="i-ic-round-format-quote" class=" size-16 rotate-3 self-start text-teal-500" />

      <div class="mx-auto w-3/4 pb-4">
        <p>Creativity is just connecting things. When you ask creative people how they did something, they feel a little guilty because they didn't really do it, they just saw something. It seemed obvious to them after a while. That's because they were able to connect experiences they've had and synthesize new things.</p>
        - <i>Steve Jobs</i>
      </div>
      <UIcon name="i-ic-round-format-quote" class=" size-16 items-end self-end text-teal-500" />
    </div>
  </div>
</template>
