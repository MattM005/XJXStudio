<script setup>
import { ref, onMounted } from 'vue'

const videos = ref([])

async function loadVideos() {
  const response = await fetch(
    "https://vimeo.com/api/v2/user208734932/videos.json"
  )

  const data = await response.json()

  videos.value = data.map((video, index) => ({
    id: index + 1,
    title: video.title,
    thumbnail: video.thumbnail_large,
    url: `https://player.vimeo.com/video/${video.id}`
  }))
}

onMounted(loadVideos)

function scrollToVideo(id) {
  const element = document.getElementById(`video-${id}`)
  if (element) {
    element.scrollIntoView({ behavior: 'smooth' })
  }
}
</script>

<template>
  <div class="container-xl text-neutral-50 mt-3">

    <!-- Collage di immagini -->
    <div class="grid grid-cols-1 sm:grid-cols-2 w-full h-full md:grid-cols-3 gap-4">
      <div
        v-for="video in videos"
        :key="video.id"
        class="cursor-pointer hover:opacity-80 transition"
        @click="scrollToVideo(video.id)"
      >
        <!-- wrapper con aspect ratio -->
        <div class="aspect-[16/9] w-full overflow-hidden rounded-lg shadow-md">
          <img
            :src="video.thumbnail"
            :alt="video.title"
            class="w-full h-full object-cover"
          />
        </div>
      </div>
      <h1 class="text-2xl pl-8 my-auto">...WORK IN PROGRESS</h1>
    </div>

    <!-- Sezione video -->
    <hr class="mt-16 border-2" />

    <div class="mt-32 space-y-10">
      <div
        v-for="video in videos"
        :key="'video-' + video.id"
        :id="'video-' + video.id"
        class="p-4 border rounded-lg shadow"
      >
        <h2 class="text-lg font-semibold mb-2">{{ video.title }}</h2>
        <iframe
          class="w-full aspect-video rounded-lg"
          :src="video.url"
          frameborder="0"
          allowfullscreen
        ></iframe>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  created() {
    var scripts = [
      "https://player.vimeo.com/api/player.js",
      "js/local.js"
    ];
    scripts.forEach(script => {
      let tag = document.createElement("script");
      tag.setAttribute("src", script);
      document.head.appendChild(tag);
    });
  }
}
</script>