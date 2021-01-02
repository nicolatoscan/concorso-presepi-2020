<template>
  <div class="home">    
    <Presepe />
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
