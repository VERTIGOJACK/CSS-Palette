<script setup>
  import { ref } from "vue";
  const image = ref("");
  const emit = defineEmits(["image-change"]);

  const onFilePicked = (event) => {
    onFileSelected(event.target.files[0]);
  };

  const onFileDropped = (event) => {
    event.preventDefault();
    onFileSelected(event.dataTransfer.files[0]);
  };

  const onFileSelected = (inputfile) => {
    const file = inputfile;
    const reader = new FileReader();

    // Check if the file is an image.
    if (file.type && !file.type.startsWith("image/")) {
      console.log("File is not an image.", file.type, file);
      return;
    }

    reader.addEventListener("load", (event) => {
      image.value = event.target.result;
      emit("image-change", image.value);
    });

    reader.readAsDataURL(file);
  };
</script>

<template>
  <div
    class="file-drop"
    @dragover="(e) => e.preventDefault()"
    @drop="onFileDropped">
    <!-- <p>Drop file here</p>
    <h1>OR</h1> -->
    <div class="pickfile">
      <input type="file" @change="onFilePicked" />
    </div>
  </div>
</template>

<style scoped>
  .file-drop {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    /* border: dashed 2px grey; */
    margin: 8px 0;
    padding: 8px 0;
    transition:all 0.3s ease-in-out;
  }

  p {
    margin: 0;
    user-select: none;
  }
  h1 {
    font-family: var(--title-font);
    font-size: larger;
    margin: 1px;
    padding: 1px;
    user-select: none;
  }
  .pickfile {
    width: 100%;
    display: flex;
    justify-content: center;
    text-align: center;
  }
  input {
    margin: 1px;
    padding: 1px;
    width: 50%;
  }
</style>
