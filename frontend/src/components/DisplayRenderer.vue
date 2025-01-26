<script setup>
// Built-In Import
import { ref, onBeforeMount} from 'vue';

/*
<---------- Camera Feature ---------->
*/

//Data

// Non-Reactive Data
const cameraUrl = 'http://localhost:5566/kvm_stream/'

// Reactive data (refs)
const isStreamLoaded = ref(true);

// Methods

// Event handlers

// Built-In events handlers

/**
 * Handles the successful loading of the camera stream image.
 * @param {none} None
 * @return {none} None
 */
 function handleImageLoad() {
  isStreamLoaded.value = true;
}

/**
 * Handles an error when loading the camera stream image.
 * @param {none} None
 * @return {none} None
 */
 function handleImageError() {
  
  // Update state when KVM stream image fails to load
  isStreamLoaded.value = false;
}

/*
<---------- Back End Connection ---------->
*/
// Data

// Reactive Data

const isLoading = ref(true);

// Lifecycle hooks
onBeforeMount(async ()=>{

isLoading.value = false;

})

</script>
 
<template>
  <div v-if="isLoading == false" class="flex justify-center items-center h-full w-full bg-black">
    <!--- Display the Video stream image if it's loaded successfully -->
    <img
      v-if="isStreamLoaded"
      class="h-full w-full"
      :src="cameraUrl"
      alt="Camera Stream"
      @load="handleImageLoad"
      @error="handleImageError"
      unselectable="on"
    />
    <!-- Display SVG image if Video stream is not loaded -->
    <img
      v-else
      class="h-full w-full"
      src="@/assets/backend_not_connected.jpg"
      alt="Camera Not Found"
      unselectable="on"
    />
  </div>
</template>
 
<style scoped>

</style>
