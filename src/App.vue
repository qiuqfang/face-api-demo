<script setup lang="ts">
import * as faceapi from "face-api.js";
import { onMounted } from "vue";

onMounted(async () => {
  console.log("mounted");
  const result = await faceapi.loadTinyFaceDetectorModel("/models");
  console.log(faceapi);

  console.log(result, "loadTinyFaceDetectorModel");

  const video = document.getElementById("face_video") as HTMLVideoElement;
  const stream: any = await navigator.mediaDevices.getUserMedia({
    audio: false,
    video: true,
  });
  try {
    video.srcObject = stream;
  } catch (error) {
    video.src = window.URL.createObjectURL(stream);
  }

  video.onloadedmetadata = async () => {
    // const detection = await faceapi.detectSingleFace(
    //   video,
    //   new faceapi.TinyFaceDetectorOptions()
    // );
  };
});
</script>

<template>
  <div class="app">
    <video id="face_video" src="" autoplay controls></video>
  </div>
</template>

<style scoped>
#face_video {
  width: 100%;
  height: 200px;
}
</style>
