<script setup>
// Built-In Import
import {onBeforeMount, onBeforeUnmount, onMounted, ref} from "vue";


// Local Imports
import VideoFrameDecoderWorker from "@/scripts/videoFrameDecoderWorker?worker";


/*
<---------- KVM Feature ---------->
*/

// Data

// Reactive data (refs)

const isKvmLoaded = ref(true);
let kvmComponent;
const decoderWorker = new VideoFrameDecoderWorker();

const SRC_IMG_X_OFFSET = 0;
const SRC_IMG_Y_OFFSET = 0;
const DST_IMG_X_OFFSET = 0;
const DST_IMG_Y_OFFSET = 0;

// Methods

// Event handlers

// Built-In events handlers

/**
 * Updates the state when a KVM stream image is loaded.
 * 
 * @function handleImageLoad
 * @returns {void}
 */
const handleImageLoad = () => {

    // Update state when KVM stream image is loaded
    isKvmLoaded.value = true;

};

/**
 * Updates the state when a KVM stream image fails to load.
 * 
 * @function handleImageError
 * @returns {void}
 */
const handleImageError = () => {

    // Update state when KVM stream image fails to load
    isKvmLoaded.value = false;

};


/*
<---------- Back End Web Socket Connection ---------->
*/

// Data

// Non-Reactive data

let kvmDisplaySocket;

// Reactive Data

// This is used to represent if this component has registered it self
// For authentication with the backend
const isLoading = ref(true);

// Lifecycle hooks

onBeforeMount(() => {  

    const kvmUrlPath = "ws://localhost:5566/websocket/kvm-stream";

    // Create a WebSocket connection
    kvmDisplaySocket = new WebSocket(kvmUrlPath);
    kvmDisplaySocket.binaryType = "arraybuffer";

    kvmDisplaySocket.onopen = () => {

        handleImageLoad();
        kvmComponent = document.getElementById("kvm-container").
            getContext("2d");
    
    };
  
    kvmDisplaySocket.onmessage = (event) => {

        decoderWorker.postMessage(
            {data: event.data,
                type: "videoData"},
            [event.data]
        );

    };

    kvmDisplaySocket.onclose = () => {

        decoderWorker.terminate();
        handleImageError();

    };

    kvmDisplaySocket.onerror = (event) => {

        console.error(
            "Error occurred:",
            event
        );

    };

    isLoading.value = false;

});

onBeforeUnmount(() => {

    decoderWorker.terminate();

    isLoading.value = true;

});

/*
<---------- KVM Rendering Feature ---------->
*/

// Data

// Reactive Data

const defaultVideoWidth = 1020;
const defaultVideoHeight = 720;

const kvmContainerParentWidth = ref(defaultVideoWidth);
const kvmContainerParentHeight = ref(defaultVideoHeight);

// Methods

// Utils

/**
 * Renders KVM display based on decoder event.
 * 
 * @param {Object} decoderEvent - Decoder event data.
 * @param {Object} [decoderEvent.data.frame] - Frame object.
 * @param {string} [decoderEvent.data.message] - Log message.
 * @returns {void}
 */
const renderKvmDisplay = (decoderEvent) => {

    if (decoderEvent.data.type === "frame") {

        kvmComponent.drawImage(
            decoderEvent.data.frame, 
            SRC_IMG_X_OFFSET, 
            SRC_IMG_Y_OFFSET, 
            decoderEvent.data.frame.displayWidth, 
            decoderEvent.data.frame.displayHeight,
            DST_IMG_X_OFFSET, 
            DST_IMG_Y_OFFSET, 
            kvmContainerParentWidth.value, 
            kvmContainerParentHeight.value
        );

        /**
     * Closes the frame to free up resources.
     */
        decoderEvent.data.frame.close();
  
    } else if (decoderEvent.data.type === "log") {

        /**
     * Logs the message to the console.
     */
        console.log(decoderEvent.data.message);
    
    }

};


/**
* Updates the width and height of the Video Display box.
*/
function updateDimensions () {

    kvmContainerParentWidth.value = window.innerWidth;
    kvmContainerParentWidth.value = window.innerHeight;

}


// Lifecycle hooks

onMounted(() => {

    decoderWorker.postMessage({type: "init"});

    // Handle messages from the worker
    decoderWorker.onmessage = renderKvmDisplay;

    updateDimensions();

});

onBeforeUnmount(() => {

    if (kvmDisplaySocket && kvmDisplaySocket.readyState === WebSocket.OPEN) {

        kvmDisplaySocket.close();
  
    }  
    decoderWorker.terminate();

});

</script>
 
<template>
  <div id="kvm-container-parent" class="flex justify-center items-center h-full w-full m-0 p-0 bg-black">
    <!---------- Display the KVM stream image if it's loaded successfully ---------->
    <canvas
      id="kvm-container"
      v-if="isKvmLoaded"
      :width="kvmContainerParentWidth"
      :height="kvmContainerParentHeight"
      alt="KVM Stream"
      @contextmenu.prevent
      unselectable="on"
    > </canvas>
    <!---------- Display SVG image if KVM stream is not loaded ---------->
    <img
      v-else
      class="h-full w-full"
      src="@/assets/backend_not_connected.jpg"
      alt="KVM Not Found"
      unselectable="on"
    />
  </div>
</template>

<style scoped>
</style>
