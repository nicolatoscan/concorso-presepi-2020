<template>
  <div class="home">
    <div v-for="(n, i) in Array(3)" :key="i" class="presepe-wrapper">
      <Presepe v-model="rat" :name="'Presepe 1'" class="presepe" />
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import presepi from "@/assets/presepi.json";
import { PresepeInfo } from "@/models/presepe-info";
import Presepe from "@/components/Presepe.vue";

export default defineComponent({
  name: "Home",
  data: function () {
    return {
      presepi: [] as PresepeInfo[],
      rat: 3
    };
  },
  components: {
    Presepe
  },
  created: function () {
    this.$data.presepi = presepi;
  },
});
</script>

<style lang="scss">
html {
  scroll-snap-type: y mandatory;
}
.home {
    scroll-snap-type: y mandatory;
    scroll-snap-align: start;

  .presepe-wrapper {
    scroll-snap-align: start;
    min-height: 80vh;
    max-height: 100vh;
    display: flex;
    align-items: center;

    .presepe {
      width: 80vw;
      max-width: 960px;
    }
  }
}
</style>
