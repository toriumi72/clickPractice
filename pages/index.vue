<script setup lang="ts">

</script>

<template>
  <div>
    <div>このボタンを素早く押せるものこそが、真の漢なり</div>
    <div class="flex items-center justify-center h-full w-full">
      <button class="w-16 h-16 ">
        <BlockIcon class="text-5xl text-red-600" name="fluent:video-clip-16-regular"></BlockIcon>
      </button>
    </div>
    <div>さあ、行け</div>
    <div class="inline-block border-black border">
      <button @click="navigateTo('/level1')">レベル１へ</button>
    </div>
  </div>
</template>

<style scoped>

</style>