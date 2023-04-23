<script setup>
  import DropFile from "./DropFile.vue";
  import { ref } from "vue";

  const image = ref(File);

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
  }
</style>
