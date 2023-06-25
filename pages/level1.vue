<script setup lang="ts">
const showScrollModal = ref(false)
const showClickModal = ref(false)
const titleElement = ref<any>(null)
const observer = ref<any>(null)

//timer
const startTime = ref<any>(0)
const elapsedTime = ref<any>(0)
let timerId: ReturnType<typeof setInterval> | null = null // Change here

// onMounted(() => {
//   startTime.value = localStorage.getItem('startTime') ?? Date.now().toString()
//   localStorage.setItem('startTime', startTime.value)

//   timerId = setInterval(() => {
//     const now = Date.now()
//     elapsedTime.value = Math.floor((now - Number(startTime.value)) / 1000)
//   }, 1000)
// })

// onBeforeUnmount(() => {
//   if (timerId) {
//     clearInterval(timerId)
//   }
// })

// const stopAndSaveResult = () => {
//   if (timerId) {
//     clearInterval(timerId)
//     localStorage.setItem('result', elapsedTime.value.toString())
//   }
// }

const onMissed = () => {
  alert('バカwwざんねん😢もう一度ww')
  window.scrollTo(0, 0)
  location.reload()
}

//1
const onCloseScrollModal = () => {
  showScrollModal.value = false
  nextStep()
}

watch(showScrollModal, (newValue) => {
  if (newValue) {
    document.body.style.overflow = 'hidden'
  } else {
    document.body.style.overflow = ''
  }
})

onMounted(() => {
  const options = {
    root: null,
    rootMargin: '0px',
    threshold: 0.5
  }

  observer.value = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
      if (entry.isIntersecting) {
        showScrollModal.value = true
        observer.value.disconnect() 
      }
    })
  }, options)

  observer.value.observe(titleElement.value)
})

onBeforeUnmount(() => {
  if (observer.value) {
    observer.value.disconnect()
  }
})

//2
const nextStep = () => {
  showClickModal.value = true
}

//Succeed
const onSucceed = () => {
  // stopAndSaveResult()
  alert('おめでとう！')
  navigateTo('/level2')
}

</script>

<template>
  <div>Elapsed time: {{ elapsedTime }} seconds</div>
  <div>
    <div class="w-full h-[80px] border">
      〇〇サイト
    </div>
    <div class="w-full h-auto">
      <div class="w-full h-[200px] border">
        広告
      </div>
      <div class="grid grid-cols-2 gap-3">
        <div v-for="(num, index) in 12" :key="index" class="w-full h-[100px] border">{{ num }}ミニ広告</div>
      </div>
      <div class="w-full">
        <div class="border">
          タイトル
        </div>
        <div class="relative flex justify-center w-full aspect-video">
          <transition name="modal">
            <div v-if="showClickModal" @click="onMissed" class="absolute top-0 right-0 w-screen h-[90px] bg-black z-[99]"></div>
          </transition>
          <div class="flex items-center justify-center h-full w-full" ref="titleElement">
            <button @click="onSucceed" class="w-16 h-16 ">
              <BlockIcon class="text-5xl text-red-600" name="fluent:video-clip-16-regular"></BlockIcon>
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="w-full h-[80px] border">
      footer
    </div>
    <ClientOnly>
      <teleport to="body">
        <transition>
          <div v-if="showScrollModal" class="fixed inset-0 m-auto w-[90vw] h-[90vh] bg-black bg-opacity-50 flex items-center justify-center rounded-md z-[99]">
            <div class="absolute top-0 right-0">
              <button @click="onCloseScrollModal" class="w-[15px] h-[15px]"></button>
            </div>
            <div @click="onMissed" class="absolute top-3 right-4">
              <BlockIcon class="text-2xl text-white" name="mingcute:close-line"></BlockIcon>
            </div>
          </div>
        </transition>
      </teleport>
    </ClientOnly>
  </div>
</template>

<style scoped>
body * {
  /* position: relative; */
}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.2s ease;
  transition-delay: 0s;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

.v-enter-to,
.v-leave-from {
  opacity: 1;
}

.modal-enter-active {
  transition: all 4.5s ease;
  transition-delay: 0.8s;
}

.modal-enter-from {
  transform: translateY(100%);
  opacity: 0;
}

.modal-enter-to {
  transform: translateY(0);
  opacity: 1;
}
</style>