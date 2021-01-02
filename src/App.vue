<template>
  <button class="reset-btn" @click="clearLocalStorage()">RESET</button>
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
  },
  methods: {
    clearLocalStorage: function() {
      localStorage.removeItem('ratings')
      location.reload()
    }
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
.reset-btn {
  position: fixed;
}

</style>
