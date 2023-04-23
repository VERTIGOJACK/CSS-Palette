<script setup>
  import PaletteItem from "./components/PaletteItem.vue";
  import FromImage from "./components/FromImage.vue";
  import { ref } from "vue";

  const colorsList = ref([""]);
  const colorPrefix = ref("palette");

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
</script>

<template>
  <div class="wrapper">
    <h1>CSS Palette</h1>
    <div class="content">
      <div class="left">
        <section class="code">
          <button class="copy">copy</button>
          <pre>{{ convertToCss() }}</pre>
        </section>
        <section class="image">
          <FromImage />
        </section>
      </div>
      <section class="maker">
        <div class="prefix">
          <label for="prefix">Color prefix:</label>
          <input type="text" name="prefix" />
        </div>
        <PaletteItem
          v-for="(item, index) in colorsList"
          :key="index"
          v-model="colorsList[index]" />
        <div class="add-color">
          <p>Add</p>
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

  .content {
    height: 100%;
  }

  .left {
    max-width: 40%;
  }

  .code,
  .image {
    height: 50%;
  }

  .code {
    background-color: whitesmoke;
  }

  .content {
    display: flex;
    justify-content: space-around;
    width: 100%;
  }
  .maker {
    overflow: scroll;
  }
</style>
