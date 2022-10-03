<script setup lang="ts">
import { onMounted, ref } from "vue";
import PopUp from "./components/PopUp.vue";

onMounted(() => {
  localStorage.setItem("emails", JSON.stringify([]));
});

const isEnabled = ref(false);
function setEnabled(val: boolean) {
  isEnabled.value = val;
}


</script>

<template>
  <div class="wrapper">
    <PopUp :is-enabled="isEnabled" :setEnabled="setEnabled" />
    <button class="button" @click="() => setEnabled(true)">Показать модальное окно</button>
  </div>
</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@500;700&display=swap");

.wrapper {
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
  background-color: lightblue;
}
.button {
  width: 400px;
  height: 300px;
  backface-visibility: hidden;
  background-color: #c24dfe;
  border-radius: 6px;
  border-width: 0;
  box-sizing: border-box;
  font-family: "Roboto", sans-serif;
  font-weight: 700;
  color: #fff;
  cursor: pointer;
  font-size: 100%;
  height: 44px;
  line-height: 1.15;
  padding: 0 25px;
  text-align: center;
  transform: translateZ(0);
  transition: all 0.2s, box-shadow 0.08s ease-in;
  box-shadow: rgba(50, 50, 93, 0.1) 0 0 0 1px inset, rgba(50, 50, 93, 0.1) 0 2px 5px 0,
    rgba(0, 0, 0, 0.07) 0 1px 1px 0;
}

.button:hover {
  box-shadow: rgba(50, 50, 93, 0.1) 0 0 0 1px inset, rgba(50, 50, 93, 0.2) 0 6px 15px 0,
    rgba(0, 0, 0, 0.1) 0 2px 2px 0, rgba(50, 151, 211, 0.3) 0 0 0 4px;
}
</style>
