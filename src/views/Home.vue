<template>
  <div class="home">
    Ciao
    <ImageSlider />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import presepi from "@/assets/presepi.json";
import { PresepeInfo } from "@/models/presepe-info";
import ImageSlider from "@/components/ImageSlider.vue";

export default defineComponent({
  name: "Home",
  data: function () {
    return {
      presepi: [] as PresepeInfo[],
    };
  },
  components: {
    ImageSlider,
  },
  created: function () {
    this.$data.presepi = presepi;
  },
});
</script>
