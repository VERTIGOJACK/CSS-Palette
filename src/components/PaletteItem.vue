<script setup>
  import ColorMenu from "./ColorMenu.vue";
  import { ref } from "vue";

  const emit = defineEmits(["update:modelValue"]);

  function test(e) {
    console.log(e);
  }

  const updateColors = () => {
    emit("update:modelValue", model.value);
  };

  const enableLight = ref(false);
  const enableDark = ref(false);

  const model = ref({
    main: "",
    dark: "",
    light: "",
    enabled: { dark: false, light: false },
  });
</script>

<template>
  <div class="palette-item">
    <div class="color">
      <ColorMenu
        :name="'Main '"
        @color="
          (e) => {
            model.main = e;
            updateColors();
          }
        "></ColorMenu>
    </div>
    <div class="colors-display">
      <div
        class="light display"
        :class="!model.enabled.light ? 'deactivated' : ''">
        <p v-if="!model.enabled.light">-</p>
      </div>
      <div class="main display"></div>
      <div
        class="dark display"
        :class="!model.enabled.dark ? 'deactivated' : ''">
        <p v-if="!model.enabled.dark">-</p>
      </div>
    </div>
    <div class="variation-light">
      <div v-if="!model.enabled.light">
        <p>light variation</p>
        <button @click="model.enabled.light = !model.enabled.light">+</button>
      </div>
      <div v-if="model.enabled.light">
        <ColorMenu
          v-if="model.enabled.light"
          name="Light "
          @color="
            (e) => {
              model.light = e;
              updateColors();
            }
          "
          :maincolor="model.main"></ColorMenu>
        <button @click="model.enabled.light = !model.enabled.light">-</button>
      </div>
    </div>
    <div class="variation-dark">
      <div v-if="!model.enabled.dark">
        <p>dark variation</p>
        <button @click="model.enabled.dark = !model.enabled.dark">+</button>
      </div>
      <div v-if="model.enabled.dark">
        <ColorMenu
          v-if="model.enabled.dark"
          name="Dark "
          @color="
            (e) => {
              model.dark = e;
              updateColors();
            }
          "
          :maincolor="model.main"></ColorMenu>
        <button @click="model.enabled.dark = !model.enabled.dark">-</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .palette-item {
    margin: 8px;
    padding: 8px;
    display: grid;
    place-items: center;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 2fr;
    border: solid 1px whitesmoke;
    border-radius: 4px;
    filter: drop-shadow(2px 2px 2px rgba(0, 0, 0, 0.2));
    background-color: white;
  }
  .colors-display {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-template-rows: 1fr;
    width: 75%;
    height: 100%;
    align-items: end;
  }
  .display {
    display: flex;
    height: 60%;
    width: 100%;
    justify-content: center;
    align-items: center;
  }

  .main {
    background-color: v-bind("model.main");
  }

  .light {
    background-color: v-bind("model.light");
  }

  .dark {
    background-color: v-bind("model.dark");
  }

  .deactivated {
    background-color: whitesmoke;
    color: gray;
    border-color: whitesmoke;
  }
</style>
