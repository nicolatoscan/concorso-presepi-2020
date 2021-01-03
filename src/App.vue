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
  },
  methods: {
    clearLocalStorage: function() {
      localStorage.removeItem('ratings2')
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
  color: white;
}

body {
  margin: 0;
}
.reset-btn {
  position: fixed;
}

.box {
  background: radial-gradient(ellipse at bottom, #FF2222 0%, #992222 100%);
  border: 1px solid #DDA700;
}

</style>
