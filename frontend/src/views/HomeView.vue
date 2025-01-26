<script setup>
import DisplayRenderer from '../components/DisplayRenderer.vue'
import H264VideoRenderer from '../components/H264VideoRenderer.vue'
</script>

<template>
  <main>
    <div class="fixed top-0 z-0 h-screen w-screen m-0 p-0 bg-black">
      <H264VideoRenderer />
    </div>
  </main>
</template>
