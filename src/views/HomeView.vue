<template>
  <div class="mt-5 container text-center">
    <WebCam />
  </div>


</template>

<script lang="ts">
import { defineComponent } from 'vue';
import WebCam from '@/components/AppWebcam.vue';

export default defineComponent({
  components: {
    WebCam
  }
});
</script>
