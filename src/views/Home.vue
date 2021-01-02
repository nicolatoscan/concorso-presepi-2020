<template>
  <div class="home">
    <div v-for="(presepe, i) in presepi" :key="i" class="presepe-wrapper">
      <Presepe v-model="ratings[presepe.name]" :name="presepe.name" :images="presepe.photos" class="presepe" />
    </div>
    <button @click="sendResults()">Invia i voti</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import presepi from "@/assets/presepi.json";
import { PresepeInfo } from "@/models/presepe-info";
import Presepe from "@/components/Presepe.vue";

const ratings: { [id: string]: (number | null)} = {}
for (const p of presepi) {
  ratings[p.name] = null;
}

export default defineComponent({
  name: "Home",
  data: function () {
    return {
      presepi: [] as PresepeInfo[],
      ratings: ratings
    };
  },
  components: {
    Presepe
  },
  created: function () {
    this.$data.presepi = presepi;
  },
  methods: {
    sendResults: function() {
      console.log(this.ratings);
    }
  }
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

  button {
    padding: 0.3em 1em;
    margin: 1em;
    margin-bottom: 5em;
    font-size: 2em;
    color: white;
    background-color: #992222;
    cursor: pointer;
    transition: 0.2s ease-in background-color;
    &:active {
      background-color: #FF2222;
    }
  }
}
</style>
