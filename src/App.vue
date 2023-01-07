<template>
  <header class="header">
    <div class="header__title">
      <h1>TODO</h1>
      <iconSunVue />
    </div>
    <div class="header__inputWrapper">
      <input class="checkbox" type="checkbox" name="" id="" />
      <input
        class="header__inputWrapper--input"
        type="text"
        placeholder="Create a new todo...."
      />
    </div>
  </header>
  <main class="content"></main>
</template>
<script lang="ts">
import { defineComponent } from "vue";
import iconMoonVue from "./components/icons/icon-moon.vue";
import iconSunVue from "./components/icons/icon-sun.vue";

export default defineComponent({
  components: { iconMoonVue, iconSunVue },
  setup() {
    return {};
  },
});
</script>
