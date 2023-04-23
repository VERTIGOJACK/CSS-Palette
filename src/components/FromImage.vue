<script setup>
  import DropFile from "./DropFile.vue";
  import { ref } from "vue";
  import vdd from "../assets/vdd.svg";

  const image = ref(File);
  image.value = vdd;

  const model = ref({});
</script>

<template>
  <div class="section-wrapper">
    <DropFile @image-change="(e) => (image = e)"></DropFile>
    <div class="image">
      <img :src="image" alt="" />
    </div>
  </div>
</template>

<style scoped>
  img {
    width: 100%;
    height: 100%;
    object-fit: contain;
    aspect-ratio: 1/1;
  }
</style>
