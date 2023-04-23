<script setup>
  import PaletteItem from "./components/PaletteItem.vue";
  import FromImage from "./components/FromImage.vue";
  import { ref } from "vue";

  const colorsList = ref([""]);
  const colorPrefix = ref("color");

  const updateCount = (operation) => {
    if (operation == "-" && colorsList.value.length > 1) {
      colorsList.value.pop("");
      convertToCss();
    } else if (operation == "+") {
      convertToCss();
      colorsList.value.push("");
    }
  };

  const convertToCss = () => {
    let cssString = ":root{\n";
    if (colorsList.value != "") {
      for (let index = 0; index < colorsList.value.length; index++) {
        const element = colorsList.value[index];
        if (element != "") {
          const indexShift = index + 1;
          cssString +=
            "--" +
            colorPrefix.value +
            "-" +
            indexShift +
            ":" +
            element.main +
            ";\n";
          if (element.enabled.light) {
            cssString +=
              "--" +
              colorPrefix.value +
              "-" +
              indexShift +
              "-light:" +
              element.light +
              ";\n";
          }
          if (element.enabled.dark) {
            cssString +=
              "--" +
              colorPrefix.value +
              "-" +
              indexShift +
              "-dark:" +
              element.dark +
              ";\n";
          }
        }
      }
    }
    cssString += "}";
    return cssString;
  };

  const addToClipboard = () => {
    // Copy the text from function
    navigator.clipboard.writeText(convertToCss());
  };
</script>

<template>
  <div class="wrapper">
    <h1 class="title">CSS root colors</h1>
    <div class="content">
      <div class="left block">
        <section class="code">
          <button @click="addToClipboard()">Copy to clipboard</button>
          <!-- <button class="copy">copy</button> -->
          <pre>{{ convertToCss() }}</pre>
        </section>

        <section class="image">
          <h2>Colors from image</h2>
          <FromImage class="from-image" />
        </section>
      </div>
      <section class="maker block">
        <div class="prefix">
          <h2>Prefix</h2>
          <input
            type="text"
            name="prefix"
            v-model="colorPrefix"
            @input="(e) => (colorPrefix = e.target.value)" />
        </div>
        <h2>Colors</h2>
        <PaletteItem
          v-for="(item, index) in colorsList"
          :key="index"
          v-model="colorsList[index]" />
        <div class="add-color">
          <h2>Add or remove</h2>
          <button @click="updateCount('+')">+</button>
          <button @click="updateCount('-')">-</button>
        </div>
      </section>
    </div>
  </div>
</template>

<style scoped>
  .wrapper {
    height: 100vh;
    width: 100vh;
  }

  .title {
    margin: 20px;
    font-family: var(--title-font) ;
    font-weight: bold;
  }

  h2{
    font-family: var(--title-font);
  }
  pre {
    overflow: scroll;
    height: 100%;
    width: 90%;
    background-color: whitesmoke;
  }
  .left {
    max-width: 40%;
    width: 100%;
  }

  .code {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 40%;
    width: 100%;
  }
  .code button {
    margin: 10px;
  }
  .image {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 50%;
    max-height: 50%;
  }

  .from-image {
    width: 90%;
  }

  .content {
    display: flex;
    justify-content: space-around;
    width: 100%;
    height: 90%;
  }
  .maker {
    overflow: scroll;
    padding: 20px;
  }

  .add-color button {
    margin: 10px;
  }
</style>
