<template>
  <router-view/>
  <Snow />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import Snow from "@/components/Snow.vue";

export default defineComponent({
  name: "App",
  components: {
    Snow
  }
});
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

body {
  margin: 0;
}

</style>
