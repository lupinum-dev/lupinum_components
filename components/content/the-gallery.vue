<script setup lang="ts">
import { onMounted, ref } from 'vue'
import { PhotoAlbum } from 'vue-photo-album'
import CustomPhotoSwipeAdaper from '../CustomPhotoSwipeAdaper.vue'

// Define props
const props = defineProps<{
  id: string
}>()

const { galleries } = await useGalleries()

const filteredGalleries = galleries.value.filter(gallery => gallery.name === props.id)

let photos

if (filteredGalleries.length > 0) {
  // Ensure photos are structured correctly
  photos = filteredGalleries.map(photo => ({
    src: `//49.13.167.134:8051/assets/${photo.filename_disk}?key=2xl`,
    srcSet: [
      {
        src: `//49.13.167.134:8051/assets/${photo.filename_disk}?key=system-large-contain`,
        width: 800,
        height: Math.round(800 / photo.width * photo.height),
      },
      {
        src: `//49.13.167.134:8051/assets/${photo.filename_disk}?system-medium-contain`,
        // calculate the height of the photo, respecting the aspect ratio, using the oriinal image height and width
        width: 300,
        height: Math.round(300 / photo.width * photo.height),
      },
      {
        src: `//49.13.167.134:8051/assets/${photo.filename_disk}?key=system-small-contain`,
        width: 64,
        height: Math.round(64 / photo.width * photo.height),
      },

    ],
    width: photo.width,
    height: photo.height,
  }))
}

// shuffle photos
photos = photos.sort(() => Math.random() - 0.5)
</script>

<template>
  <div class="not-prose mt-12">
    <!-- Render your gallery details here -->

    <!-- <div v-for="photo in galleries">
      1 <img :src="`//plankton-app-97kt2.ondigitalocean.app/assets/${photo.filename_disk}` " :alt="photo.filename_download" :width="photo.width" :height="photo.height">
    </div> -->

    <PhotoAlbum :photos="photos" layout="rows" :photo-renderer="CustomPhotoSwipeAdaper" />
  <!-- Add more details as needed -->
  </div>
</template>
