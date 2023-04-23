<script setup>
  import { ref, onMounted, computed, reactive } from "vue";

  const props = defineProps(["name", "maincolor"]);
  const emit = defineEmits(["color"]);

  const color = ref("#000000");

  const calculateColor = () => {
    color.value = color.value.replace(/#/g, "");
    color.value = "#" + color.value;
    emit("color", color.value);
  };

  onMounted(() => {
    if (props.maincolor != null || "") {
      color.value = props.maincolor;
    }
    emit("color", color.value);
  });
</script>

<template>
  <div class="menu">
    <label for="color">{{ $props.name }}</label>
    <div class="line">
      <input type="text" name="color" v-model="color" @input="calculateColor" />
      <input type="color" v-model="color" @input="calculateColor" />
    </div>
  </div>
</template>

<style scoped>
  .line {
    height: 30px;
    width: 100%;
    display: flex;
    justify-content: center;
  }

  label {
    font-family: var(--title-font);
  }

  input {
    border: 0;
    padding: 0;
    margin: 0;
    height: 100%;
    font-family: var(--paragraph-font);
    border-radius: 4px 0 0 4px;
  }

  input[type="text"] {
    width: 50%;
    background-color: whitesmoke;
  }

  input::-webkit-color-swatch-wrapper {
    width: 100%;
    padding: 0;
    margin: 0;
    background: none;
  }
  input::-webkit-color-swatch {
    width: 100%;
    border: solid 1px;
    border-radius: 0 4px 4px 0;
  }
</style>
