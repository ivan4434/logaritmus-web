<script setup>
  import WavyBackground from './components/WavyBackground.vue';

  const buttons = [
    {
      label: "Versión web",
      icon: "fa-solid fa-globe",
      href: "https://logaritmus.prxdev.es/"
    },
    {
      label: "Descargar para Android",
      icon: "fa-brands fa-android",
      href: "/logaritmus-web/download/android/logaritmus.apk"
    },
    {
      label: "Descargar para Windows",
      icon: "fa-brands fa-windows",
      href: "/logaritmus-web/download/windows/Logaritmus-win.zip"
    },
    {
      label: "Descargar para Linux",
      icon: "fa-brands fa-linux",
      href: "/logaritmus-web/download/linux/Logaritmus-linux.zip"
    },
  ]

</script>

<template>
  <div class="flex flex-col w-screen h-screen bg-neutral-900 overflow-hidden text-white">

    <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 flex flex-col items-center gap-8 z-10 w-screen md:w-150 px-5">
      <img src="@/assets/logo.png" alt="" class="size-60">
      <h1 class="text-3xl font-bold">Logaritmus</h1>
      <p class="text-neutral-400 text-center">Descarga Logaritmus y descubre que las matemáticas también pueden ser divertidas</p>
      <div class="grid grid-cols-2 gap-5">
        <a v-for="btn in buttons" class="
          bg-[#0D47A1FF] hover:bg-[#2196F3FF]
          duration-200 p-5 py-2 rounded-full cursor-pointer
          active:bg-[#0B3C91FF] flex flex-row gap-5 items-center justify-center
        " :href="btn.href">
          <i :class="btn.icon"></i>
          <div>{{btn.label}}</div>
        </a>
      </div>
    </div>

    <div class="absolute top-0 left-0 right-0 opacity-70">
      <WavyBackground 
        :colors="['#D32F2FFF', '#FF6F61FF']" 
        :speed="5000"
      />
    </div>
    <div class="absolute bottom-0 left-0 right-0 rotate-180 opacity-70">
      <WavyBackground 
        :colors="['#0D47A1FF', '#2196F3FF']" 
        :speed="5000"
      />
    </div>
  </div>
</template>

<style scoped></style>
