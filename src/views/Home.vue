<script setup>
import { ref, computed, onMounted } from 'vue'
import emailjs from 'emailjs-com'

import { Swiper, SwiperSlide } from 'swiper/vue'
import 'swiper/css'
import 'swiper/css/free-mode'
import 'swiper/css/navigation'
import 'swiper/css/thumbs'
import { FreeMode, Navigation, Thumbs } from 'swiper/modules'

import { inject } from 'vue'
const globalData = inject('globalData')

/* ---------------- VIDEO ---------------- */
const videos = ref([])

async function loadVideos() {
  const response = await fetch(
    "https://vimeo.com/api/v2/user208734932/videos.json"
  )

  const data = await response.json()

  videos.value = data.map((video, index) => ({
    id: index,
    title: video.title,
    thumbnail: video.thumbnail_large,
    url: `https://player.vimeo.com/video/${video.id}`
  }))
}

/* ---------------- RECENSIONI ---------------- */
const recensioni = ref([
  {
    id: 1,
    nome: "Marco Rossi",
    voto: 5,
    testo: "Servizio impeccabile, super consigliato!"
  },
  {
    id: 2,
    nome: "Laura Bianchi",
    voto: 4,
    testo: "Ottima esperienza, personale molto disponibile."
  },
  {
    id: 3,
    nome: "Giulio Verdi",
    voto: 5,
    testo: "Qualità davvero alta, tornerò sicuramente."
  },
  {
    id: 4,
    nome: "Giulia Gatto",
    voto: 4,
    testo: "Video eccelsi proprio come li volevo."
  },
  {
    id: 5,
    nome: "Massimiliano Neri",
    voto: 4,
    testo: "Consiglio!!"
  },
  {
    id: 6,
    nome: "Mario R.",
    voto: 5,
    testo: "Ogni video è un'opera d'arte"
  },
  {
    id: 7,
    nome: "Matteo M.",
    voto: 4,
    testo: "Qualità top, bei montaggi."
  }
])

// filtro (facoltativo)
const filtroVoto = ref('')

// recensioni filtrate
const recensioniFiltrate = computed(() => {
  if (!filtroVoto.value) return recensioni.value
  return recensioni.value.filter(r => r.voto == filtroVoto.value)
})

/* ---------------- SWIPER ---------------- */
const thumbsSwiper = ref(null)
const mainSwiper = ref(null)
const currentIndex = ref(0)

const thumbs = computed(() => ({ swiper: thumbsSwiper.value }))

const setThumbsSwiper = (swiper) => (thumbsSwiper.value = swiper)

const setMainSwiper = (swiper) => {
  mainSwiper.value = swiper
  currentIndex.value = swiper?.activeIndex ?? 0
}

const onSlideChange = (swiper) => (currentIndex.value = swiper.activeIndex)

/* ---------------- INIT ---------------- */
onMounted(() => {
  loadVideos()
  // loadRecensioni()
})

/*
// ex API recensioni
async function loadRecensioni() {
  const res = await fetch('/api/recensioni')
  recensioni.value = await res.json()
}
*/
</script>

<template>
  <div class="min-h-screen w-full flex flex-col">
    <div class="container w-full h-full min-h-screen flex mt-6 justify-center flex-col mx-auto">
      <!-- HERO PAGE -->
      <div class="min-h-screen h-full">
        <!-- <div class="content max-w-4xl mx-auto">
          <h1 class="font-black title-raduno mb-3 uppercase">OUR PRODUCTIONS</h1>
          <p class="font-regular text-lg max-w-3xl my-4">
            "Checkout last video of the Scarbmafia's raduno"
          </p>
          <a href="https://www.instagram.com/xjxvisuals/" target="_blank" class="link-insta">
            <button id="myBtn" class="flex justify-center items-center text-[#da2228] gap-2 text-sm mt-6 py-3 px-4">
              VIEW VIDEO
              <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-arrow-right-bar" width="28"
                height="28" viewBox="0 0 24 24" stroke-width="1.5" stroke="#da2228" fill="none" stroke-linecap="round"
                stroke-linejoin="round">
                <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                <path d="M18 15l3 -3l-3 -3" />
                <path d="M3 12h18" />
                <path d="M3 9v6" />
              </svg>
            </button>
          </a>
        </div> -->
        <div class="content m-auto max-w-4xl w-full">
          <h1 class="title text-3xl sm:text-4xl xl:text-5xl">Scroll below</h1>
          <a @click.prevent="scrollToSection('down')" class="down-arrow smooth-scroll">
            <div class="arrow arrow-first"></div>
            <div class="arrow arrow-second"></div>
          </a>
        </div>
        <!-- freccia per andare giù -->
        <!-- <a href="#down" class="down-arrow">
          <svg xmlns="http://www.w3.org/2000/svg" width="34" height="34" fill="currentColor"
            class="bi bi-arrow-down-circle" viewBox="0 0 16 16">
            <path fill-rule="evenodd"
              d="M1 8a7 7 0 1 0 14 0A7 7 0 0 0 1 8m15 0A8 8 0 1 1 0 8a8 8 0 0 1 16 0M8.5 4.5a.5.5 0 0 0-1 0v5.793L5.354 8.146a.5.5 0 1 0-.708.708l3 3a.5.5 0 0 0 .708 0l3-3a.5.5 0 0 0-.708-.708L8.5 10.293z" />
          </svg>
        </a> -->
      </div>

      <!-- content of all the home -->
      <div
        class="container min-h-full sticky w-full max-w-4xl px-4 md:mt-24 mx-auto flex flex-col justify-center items-center">
        <div class="relative min-h-screen w-full" id="down"> <!--scroll down THERE-->
          <!-- LITTLE PREVIEW OF WORKS -->
          <h1 class="text-2xl sm:text-3xl lg:text-4xl text-center font-black">
            PREVIOUS PRODUCTIONS:
          </h1>
          <!-- MAIN SWIPER -->
          <Swiper
            :modules="[FreeMode, Navigation, Thumbs]"
            :spaceBetween="5"
            :navigation="true"
            :thumbs="thumbs"
            class="mySwiper2"
            @swiper="setMainSwiper"
            @slideChange="onSlideChange"
          >
            <SwiperSlide v-for="video in videos" :key="video.id">
              <iframe
                class="iframe-sw"
                :src="video.url"
                width="650"
                height="545"
                frameborder="0"
                allow="autoplay; fullscreen; picture-in-picture"
                allowfullscreen
              ></iframe>
            </SwiperSlide>
          </Swiper>

          <!-- THUMBNAILS SWIPER -->
          <Swiper
            :modules="[FreeMode, Navigation, Thumbs]"
            :spaceBetween="5"
            :slidesPerView="4"
            :freeMode="true"
            :watchSlidesProgress="true"
            class="mySwiper"
            @swiper="setThumbsSwiper"
          >
            <SwiperSlide v-for="video in videos" :key="'thumb-'+video.id">
              <img :src="video.thumbnail" :alt="video.title" />
            </SwiperSlide>
          </Swiper>
        </div>

        <div class="relative min-h-screen w-full flex items-center justify-center">
          <!-- SERVICES -->
          <div class="container mx-auto mt-44 mb-3">
            <h1 class="text-2xl sm:text-4xl md:text-5xl font-black w-fit mx-auto mb-12">
              DO YOU WANT A PROJECT FOR YOU?
            </h1>
            <!-- EMAIL FORM -->
            <form id="form" ref="form" @submit.prevent="sendEmail"
              class="mx-auto bg-white text-black p-8 rounded-2xl shadow-lg w-full max-w-[567px] space-y-5">
              <div class="field flex flex-col">
                <label for="email" class="font-semibold mb-1">Email</label>
                <input type="email" name="email" id="email" v-model="email" required placeholder="mario.rossi@email.com"
                  class="border border-neutral-400 rounded-lg p-2 focus:outline-none focus:ring-2 focus:ring-red-400" />
              </div>

              <div class="field flex flex-col">
                <label for="title" class="font-semibold mb-1">Title</label>
                <input type="text" name="title" id="title" v-model="title" required placeholder="Project title"
                  class="border border-neutral-400 rounded-lg p-2 focus:outline-none focus:ring-2 focus:ring-red-400" />
              </div>

              <div class="field flex flex-col">
                <label for="name" class="font-semibold mb-1">Name</label>
                <input type="text" name="name" id="name" v-model="name" required placeholder="Mario Rossi"
                  class="border border-neutral-400 rounded-lg p-2 focus:outline-none focus:ring-2 focus:ring-red-400" />
              </div>

              <div class="field flex flex-col">
                <label for="phone" class="font-semibold mb-1">Phone</label>
                <input type="tel" name="phone" id="phone" v-model="phone" required placeholder="+39 333 1234567"
                  class="border border-neutral-400 rounded-lg p-2 focus:outline-none focus:ring-2 focus:ring-red-400" />
              </div>

              <div class="field hidden">
                <label for="time" class="font-semibold mb-1">Time</label>
                <input type="text" name="time" id="time" v-model="time" readonly
                  class="border border-neutral-400 rounded-lg p-2 bg-neutral-100 cursor-not-allowed text-neutral-600" />
              </div>

              <div class="field flex flex-col">
                <label for="message" class="font-semibold mb-1">Message</label>
                <input type="text" name="message" id="message" v-model="message" required
                  placeholder="Tell us about your project..."
                  class="border border-neutral-400 rounded-lg p-2 focus:outline-none focus:ring-2 focus:ring-red-400" />
              </div>

              <!-- Submit Button -->
              <input type="submit" id="button" :value="sending ? 'Sending...' : sent ? 'Sent ✅' : 'Send Email'" :class="[
                'w-full py-3 text-lg font-bold rounded-lg transition duration-300 cursor-pointer',
                sending ? 'bg-red-400 text-white' : sent ? 'bg-green-500 text-white' : 'bg-red-600 text-white hover:bg-red-700'
              ]" :disabled="sending" />
            </form>
          </div>
        </div>

        <div class="relative min-h-screen w-full flex items-start justify-center flex-col">
          <!-- TESTIMONIALS -->

          <!-- TrustBox widget - Review Collector -->
          <div class="trustpilot-widget mx-auto mt-28 mb-12" data-locale="it-IT" data-template-id="56278e9abfbbba0bdcd568bc" data-businessunit-id="6a268d98d088a85967cd1c64" data-style-height="75px" data-style-width="100%" data-token="06cfedf1-57d6-4598-84e6-3ee1b1024bd6">
            <a href="https://it.trustpilot.com/review/xjxvisuals.com" target="_blank" rel="noopener">Trustpilot</a>
          </div>
          <!-- End TrustBox widget -->

          <!-- <div class="container mx-auto max-w-[717px] left-0 md:mb-8 relative tst">
            <h1 class=" text-2xl sm:text-3xl xl:text-4xl font-black mt-32 mb-8 w-fit">TESTIMONIALS</h1>
            <Swiper
              :modules="[Navigation]"
              :slides-per-view="3"
              :space-between="20"
              navigation
              :breakpoints="{
                0: { slidesPerView: 1 },
                640: { slidesPerView: 2 },
                1024: { slidesPerView: 3 }
              }"
            >
              <SwiperSlide v-for="recensione in recensioni" :key="recensione.id">
                <div class="review p-4 border rounded-lg text-center">
                  <p class="font-bold">{{ recensione.nome }}</p>
                  <div>
                    <span v-for="n in recensione.voto" :key="n">⭐</span>
                  </div>
                  <p class="max-w-[150px] mx-auto">{{ recensione.testo }}</p>
                </div>
              </SwiperSlide>
            </Swiper>
          </div> -->

          <!-- CONTACT -->
          <div class="container mx-auto max-w-[717px] mt-2 md:mb-6">
            <h1 class=" text-2xl sm:text-3xl xl:text-4xl font-black mt-12 mb-8 w-fit">GET IN TOUCH!</h1>
            <div
              class="flex justify-between items-center gap-2 flex-wrap sm:flex-nowrap mt-8 w-fit max-w-[400px] mb-16">
              <a :href="$globalData.email">
                <button id="myBtn" class="py-4 min-w-full flex justify-center items-center gap-2 font-black uppercase">
                  email
                  <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                    class="bi bi-envelope-arrow-up-fill" viewBox="0 0 16 16">
                    <path
                      d="M.05 3.555A2 2 0 0 1 2 2h12a2 2 0 0 1 1.95 1.555L8 8.414zM0 4.697v7.104l5.803-3.558zm.192 8.159 6.57-4.027L8 9.586l1.239-.757.367.225A4.49 4.49 0 0 0 8 12.5c0 .526.09 1.03.256 1.5H2a2 2 0 0 1-1.808-1.144M16 4.697v4.974A4.5 4.5 0 0 0 12.5 8a4.5 4.5 0 0 0-1.965.45l-.338-.207z" />
                    <path
                      d="M12.5 16a3.5 3.5 0 1 0 0-7 3.5 3.5 0 0 0 0 7m.354-5.354 1.25 1.25a.5.5 0 0 1-.708.708L13 12.207V14a.5.5 0 0 1-1 0v-1.717l-.28.305a.5.5 0 0 1-.737-.676l1.149-1.25a.5.5 0 0 1 .722-.016" />
                  </svg>
                </button>
              </a>
              <a :href="$globalData.tel">
                <button id="myBtn" itemprop="telephone"
                  class="sm:ml-4 sm:mt-0 py-4 min-w-full flex justify-center items-center gap-2 font-black uppercase w-[115%]">
                  phone number
                  <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                    class="bi bi-telephone-forward-fill" viewBox="0 0 16 16">
                    <path fill-rule="evenodd"
                      d="M1.885.511a1.745 1.745 0 0 1 2.61.163L6.29 2.98c.329.423.445.974.315 1.494l-.547 2.19a.68.68 0 0 0 .178.643l2.457 2.457a.68.68 0 0 0 .644.178l2.189-.547a1.75 1.75 0 0 1 1.494.315l2.306 1.794c.829.645.905 1.87.163 2.611l-1.034 1.034c-.74.74-1.846 1.065-2.877.702a18.6 18.6 0 0 1-7.01-4.42 18.6 18.6 0 0 1-4.42-7.009c-.362-1.03-.037-2.137.703-2.877zm10.761.135a.5.5 0 0 1 .708 0l2.5 2.5a.5.5 0 0 1 0 .708l-2.5 2.5a.5.5 0 0 1-.708-.708L14.293 4H9.5a.5.5 0 0 1 0-1h4.793l-1.647-1.646a.5.5 0 0 1 0-.708" />
                  </svg>
                </button>
              </a>
            </div>
          </div>
          
          <!-- SOCIALS -->
          <!-- PREVIEW SOCIALS (EX. INSTAGRAM PROFILE) -->
          <div class="container mx-auto max-w-[717px] mt-2 md:mb-24">
            <!-- socials -->
            <h1 class=" text-2xl sm:text-3xl xl:text-4xl font-black mt-16 pt-4 mb-8">Socials: </h1>
            <div class="social-wrap flex gap-6 text-sm mt-2">
              <a :href="$globalData.socialLinks.instagram" target="_blank" class="social-icon">
                <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" fill="currentColor"
                  class="bi bi-instagram" viewBox="0 0 16 16">
                  <path
                    d="M8 0C5.829 0 5.556.01 4.703.048 3.85.088 3.269.222 2.76.42a3.9 3.9 0 0 0-1.417.923A3.9 3.9 0 0 0 .42 2.76C.222 3.268.087 3.85.048 4.7.01 5.555 0 5.827 0 8.001c0 2.172.01 2.444.048 3.297.04.852.174 1.433.372 1.942.205.526.478.972.923 1.417.444.445.89.719 1.416.923.51.198 1.09.333 1.942.372C5.555 15.99 5.827 16 8 16s2.444-.01 3.298-.048c.851-.04 1.434-.174 1.943-.372a3.9 3.9 0 0 0 1.416-.923c.445-.445.718-.891.923-1.417.197-.509.332-1.09.372-1.942C15.99 10.445 16 10.173 16 8s-.01-2.445-.048-3.299c-.04-.851-.175-1.433-.372-1.941a3.9 3.9 0 0 0-.923-1.417A3.9 3.9 0 0 0 13.24.42c-.51-.198-1.092-.333-1.943-.372C10.443.01 10.172 0 7.998 0zm-.717 1.442h.718c2.136 0 2.389.007 3.232.046.78.035 1.204.166 1.486.275.373.145.64.319.92.599s.453.546.598.92c.11.281.24.705.275 1.485.039.843.047 1.096.047 3.231s-.008 2.389-.047 3.232c-.035.78-.166 1.203-.275 1.485a2.5 2.5 0 0 1-.599.919c-.28.28-.546.453-.92.598-.28.11-.704.24-1.485.276-.843.038-1.096.047-3.232.047s-2.39-.009-3.233-.047c-.78-.036-1.203-.166-1.485-.276a2.5 2.5 0 0 1-.92-.598 2.5 2.5 0 0 1-.6-.92c-.109-.281-.24-.705-.275-1.485-.038-.843-.046-1.096-.046-3.233s.008-2.388.046-3.231c.036-.78.166-1.204.276-1.486.145-.373.319-.64.599-.92s.546-.453.92-.598c.282-.11.705-.24 1.485-.276.738-.034 1.024-.044 2.515-.045zm4.988 1.328a.96.96 0 1 0 0 1.92.96.96 0 0 0 0-1.92m-4.27 1.122a4.109 4.109 0 1 0 0 8.217 4.109 4.109 0 0 0 0-8.217m0 1.441a2.667 2.667 0 1 1 0 5.334 2.667 2.667 0 0 1 0-5.334" />
                </svg>
              </a>
              <a :href="$globalData.socialLinks.whatsapp" itemscope itemtype="http://schema.org/LocalBusiness" class="social-icon">
                <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" fill="currentColor"
                  class="bi bi-whatsapp" viewBox="0 0 16 16">
                  <path
                    d="M13.601 2.326A7.85 7.85 0 0 0 7.994 0C3.627 0 .068 3.558.064 7.926c0 1.399.366 2.76 1.057 3.965L0 16l4.204-1.102a7.9 7.9 0 0 0 3.79.965h.004c4.368 0 7.926-3.558 7.93-7.93A7.9 7.9 0 0 0 13.6 2.326zM7.994 14.521a6.6 6.6 0 0 1-3.356-.92l-.24-.144-2.494.654.666-2.433-.156-.251a6.56 6.56 0 0 1-1.007-3.505c0-3.626 2.957-6.584 6.591-6.584a6.56 6.56 0 0 1 4.66 1.931 6.56 6.56 0 0 1 1.928 4.66c-.004 3.639-2.961 6.592-6.592 6.592m3.615-4.934c-.197-.099-1.17-.578-1.353-.646-.182-.065-.315-.099-.445.099-.133.197-.513.646-.627.775-.114.133-.232.148-.43.05-.197-.1-.836-.308-1.592-.985-.59-.525-.985-1.175-1.103-1.372-.114-.198-.011-.304.088-.403.087-.088.197-.232.296-.346.1-.114.133-.198.198-.33.065-.134.034-.248-.015-.347-.05-.099-.445-1.076-.612-1.47-.16-.389-.323-.335-.445-.34-.114-.007-.247-.007-.38-.007a.73.73 0 0 0-.529.247c-.182.198-.691.677-.691 1.654s.71 1.916.81 2.049c.098.133 1.394 2.132 3.383 2.992.47.205.84.326 1.129.418.475.152.904.129 1.246.08.38-.058 1.171-.48 1.338-.943.164-.464.164-.86.114-.943-.049-.084-.182-.133-.38-.232" />
                </svg>
              </a>
              <a :href="$globalData.socialLinks.facebook" target="_blank" class="social-icon">
                <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" fill="currentColor"
                  class="bi bi-facebook" viewBox="0 0 16 16">
                  <path
                    d="M16 8.049c0-4.446-3.582-8.05-8-8.05C3.58 0-.002 3.603-.002 8.05c0 4.017 2.926 7.347 6.75 7.951v-5.625h-2.03V8.05H6.75V6.275c0-2.017 1.195-3.131 3.022-3.131.876 0 1.791.157 1.791.157v1.98h-1.009c-.993 0-1.303.621-1.303 1.258v1.51h2.218l-.354 2.326H9.25V16c3.824-.604 6.75-3.934 6.75-7.951" />
                </svg>
              </a>
              <a :href="$globalData.socialLinks.vimeo" target="_blank" class="social-icon">
                <svg xmlns="http://www.w3.org/2000/svg" width="48" height="48" fill="currentColor" class="bi bi-vimeo"
                  viewBox="0 0 16 16">
                  <path
                    d="M15.992 4.204q-.106 2.334-3.262 6.393-3.263 4.243-5.522 4.243-1.4 0-2.367-2.583L3.55 7.523Q2.83 4.939 2.007 4.94q-.178.001-1.254.754L0 4.724a210 210 0 0 0 2.334-2.081q1.581-1.364 2.373-1.437 1.865-.185 2.298 2.553.466 2.952.646 3.666.54 2.447 1.186 2.445.5 0 1.508-1.587 1.006-1.587 1.077-2.415.144-1.37-1.077-1.37a3 3 0 0 0-1.185.261q1.183-3.86 4.508-3.756 2.466.075 2.324 3.2z" />
                </svg>
              </a>
            </div>
          </div>
        </div>
      <!-- footer in the app.vue -->
    </div>
  </div>
  </div>
</template>