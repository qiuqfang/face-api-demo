<script setup lang="ts">
import * as faceapi from "face-api.js";
import { onMounted } from "vue";

onMounted(async () => {
  console.log("mounted");
  const result = await faceapi.loadTinyFaceDetectorModel("/models");
  console.log(faceapi);

  console.log(result, "loadTinyFaceDetectorModel");

  const videoEl = document.getElementById("face_video") as HTMLVideoElement;

  const stream: any = await navigator.mediaDevices.getUserMedia({
    audio: false,
    video: {},
  });
  try {
    videoEl.srcObject = stream;
  } catch (error) {
    videoEl.src = window.URL.createObjectURL(stream);
  }

  const onPlay = async () => {
    console.log(videoEl.videoWidth, videoEl.videoHeight);

    const detection = await faceapi.detectSingleFace(
      videoEl,
      new faceapi.TinyFaceDetectorOptions({
        inputSize: 224,
        scoreThreshold: 0.5,
      })
    );
    console.log(detection);
    // setTimeout(() => onPlay());
  };
  videoEl.onloadedmetadata = onPlay;
});
</script>

<template>
  <div class="app">
    <video id="face_video" src="" autoplay></video>
  </div>
</template>

<style scoped>
#face_video {
  width: 100vw;
  height: 100vh;
}
</style>
