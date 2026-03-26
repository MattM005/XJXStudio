<script setup>
  import { inject } from 'vue'
  const globalData = inject('globalData')
</script>

<template>
  <div class="container z-[999] sticky w-full min-h-screen max-w-4xl px-4 mt-20 mx-auto">
    <div class="flex flex-col bg-white shadow-sm mx-auto border border-slate-200 rounded-lg my-6 min-h-[450px] max-w-[550px]">
      <div class="m-2.5 overflow-hidden rounded-md max-h-80 flex justify-center items-center">
        <img class="object-cover" width="150" height="150" src="../../public/logo.jpeg" alt="profile-picture" />
      </div>
      <div class="p-6 text-center">
        <h4 class="mb-1 text-2xl font-semibold text-slate-800">
          Versace Francesco
        </h4>
        <p class="text-base text-slate-600 mt-4 font-medium">
          Born in 2004, I am a boy from Monfalcone (GO) who has always liked recording and editing videos, a passion that has today become a solid reality; in fact, since 2025 I have opened my own company with IVA number, taking on increasingly important tasks with increasingly famous people.
        </p>
        <p class="text-base text-slate-900 mt-4 font-light font-impact">
          <i>"To make a video well, you not only need experience and knowledge, but also a lot of inspiration and creativity"</i>
        </p>
      </div>
      <div class="flex justify-center p-6 pt-2 gap-7">
        <a :href="$globalData.socialLinks.instagram" target="_blank">
          <button class="min-w-32  rounded-md bg-slate-800 py-2 px-4 border border-transparent text-center text-sm text-white transition-all shadow-md hover:shadow-lg focus:bg-slate-700 focus:shadow-none active:bg-slate-700 hover:bg-slate-700 active:shadow-none disabled:pointer-events-none disabled:opacity-50 disabled:shadow-none" type="button">
            Follow
          </button>
        </a>
      </div>
    </div>
  </div>
</template>

