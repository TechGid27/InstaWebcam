<template>
  <div>
    <video ref="videoRef" autoplay playsinline width="530" height="400" class="rounded shadow mb-3 img-fluid"></video>
    <canvas ref="canvasRef" style="display: none;"></canvas>
   


    
    <div class="mb-3">
      <button @click="startCamera" class="btn btn-primary me-2">Start Camera</button>
      <button @click="capturePhoto" class="btn btn-success">Capture</button>
    </div>

    <div v-if="capturedImage" class="pb-5">
      <h5>Captured Image:</h5>
      <img :src="capturedImage" alt="Captured Image" class="img-thumbnail" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onBeforeUnmount } from 'vue';

export default defineComponent({
  name: 'AppWebcam',
  setup() {
    const videoRef = ref<HTMLVideoElement | null>(null);
    const canvasRef = ref<HTMLCanvasElement | null>(null);
    const capturedImage = ref<string | null>(null);
    let stream: MediaStream | null = null;

    const startCamera = async () => {
      try {
        stream = await navigator.mediaDevices.getUserMedia({ video: true });
        if (videoRef.value) {
          videoRef.value.srcObject = stream;
        }
      } catch (error) {
        console.error('Webcam access denied:', error);
      }
    };

    const capturePhoto = () => {
      if (!videoRef.value || !canvasRef.value) return;

      const video = videoRef.value;
      const canvas = canvasRef.value;
      const context = canvas.getContext('2d');

      if (context) {
        canvas.width = video.videoWidth;
        canvas.height = video.videoHeight;
        context.drawImage(video, 0, 0, canvas.width, canvas.height);
        capturedImage.value = canvas.toDataURL('image/png');
      }
    };

    onBeforeUnmount(() => {
      stream?.getTracks().forEach((track) => track.stop());
    });

    return {
      videoRef,
      canvasRef,
      capturedImage,
      startCamera,
      capturePhoto
    };
  }
});
</script>
