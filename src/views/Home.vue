<template>
  <div class="home">
    <div v-for="(presepe, i) in presepi" :key="i" class="presepe-wrapper">
      <Presepe v-model="ratings[presepe.code]" :name="presepe.name" :images="presepe.photos" class="presepe" />
    </div>
    <button @click="sendResults()" :disabled="!submitBtnInfo().enabled">
      {{submitBtnInfo().text}}
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import presepi from "@/assets/presepi.json";
import { PresepeInfo } from "@/models/presepe-info";
import Presepe from "@/components/Presepe.vue";
import axios from 'axios';

const ratings: { [id: string]: (number | null)} = {}
for (const p of presepi) {
  ratings[p.code] = null;
}

export default defineComponent({
  name: "Home",
  data: function () {
    return {
      presepi: [] as PresepeInfo[],
      ratings: ratings,
      sending: false,
      sent: false
    };
  },
  components: {
    Presepe
  },
  created: function () {
    this.$data.presepi = presepi;
    if (localStorage.getItem('ratings'))
      this.sent = true
  },
  methods: {
    sendResults: async function() {
      console.log(this.ratings);
      this.sending = true
      try {
        const response = await axios.post(process.env.VUE_APP_API_HOST + 'save', this.ratings)
        localStorage.setItem('ratings', JSON.stringify(this.ratings))
        this.sent = true
      } catch (error) {
        alert(`C'è stato un errore nell'invio di dati. Codice errore: ${error.response.data}`);
        this.sending = false
      }
    },
    submitBtnInfo: function(): { enabled: boolean; text: string } {
      if (this.sending)
        return { enabled: false, text: 'Inviando i voti...' }
      if (this.sent)
        return { enabled: false, text: 'I tuoi voti sono stati salvati' }

      for (const k in this.ratings) {
        if (typeof ratings[k] !== 'number') {
          return { enabled: false, text: 'Vota tutti i presepi per inviare i voti' }
        }
      }
      return { enabled: true, text: 'Invia i tuoi voti' }
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
    &:disabled {
      background-color: #888;
    }
  }
}
</style>
