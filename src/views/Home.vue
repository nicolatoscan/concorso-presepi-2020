<template>
  <div class="home">
    Ciao
    <ImageSlider />
    <StarRating v-model="rat" />
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import presepi from "@/assets/presepi.json";
import { PresepeInfo } from "@/models/presepe-info";
import ImageSlider from "@/components/ImageSlider.vue";
import StarRating from "@/components/StarRating.vue";

export default defineComponent({
  name: "Home",
  data: function () {
    return {
      presepi: [] as PresepeInfo[],
      rat: 3
    };
  },
  components: {
    ImageSlider, StarRating
  },
  created: function () {
    this.$data.presepi = presepi;
  },
});
</script>
