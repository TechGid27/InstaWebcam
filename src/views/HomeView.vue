<template>

  <div class="continer text-center mt-4 px-3">
      <WebCam/>
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