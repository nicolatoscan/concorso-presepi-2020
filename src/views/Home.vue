<template>
  <div class="home">
    <header class="box">
      <h1>Concorso presepi Natale 2021</h1>
      <div class="introduction-text">
        <p>Carissimi Parrocchiani,<br>
            siamo tutti invitati a votare questi bellissimi presepi per il nostro “concorso presepi 2021”.<br>
            Non è una gara, quanto un modo per sentirci ancora più vicini e di condivisione in un momento come questo. Non esistono presepi belli o brutti. Ognuno è originale e speciale.
            In ogni presepe vediamo e gustiamo la tenerezza di Dio che si fa bambino e viene nella nostra vita in umiltà e semplicità.
            Ringraziamo tutte le persone che hanno partecipato per la condivisione.
        </p>
        <p><b>Alcune regolette per la votazione:</b></p>
        <ol>
          <li>Perché il sondaggio vada a buon fine è necessario votare TUTTI i presepi;</li>
          <li>Sotto ogni presepe ci saranno 5 stelline – dovrete votare con le corrispondenti stelle ogni presepe da 1 a 5;</li>
          <li>Alla fine del sondaggio verrà sommato il conteggio totale di ogni presepe e saranno poi premiati i primi 3 vincitori domenica 9 gennaio dopo la Santa Messa delle ore 11.00 nella nostra chiesa Parrocchiale, dove verranno proiettati i nostri presepi alla comunità.</li>
        </ol>
        <p>Attenzione che per quanto riguarda alcuni presepi ci sono più di una foto! Usa le frecce per vederle tutte.</p>
        <p>Grazie anche a chi parteciperà a questa votazione!!</p>
      </div>
    </header>
    <div v-for="(presepe, i) in presepi" :key="i" class="presepe-wrapper">
      <Presepe v-model="ratings[presepe.code]"
        :name="presepe.name"
        :description="presepe.description"
        :images="presepe.photos"
        :isvideo="presepe.isvideo ?? false"
        class="presepe"
        :enabled="!sent" />
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
    const oldRatings = localStorage.getItem('ratings2021')
    if (oldRatings) {
      this.sent = true
      const parsedOldRatings = JSON.parse(oldRatings)
      for (const k in parsedOldRatings) {
        this.ratings[k] = parsedOldRatings[k]
      }
    } else {
      window.onbeforeunload = function() {
        return 'Non hai ancora inviato i risultati, sei sicuro di voler chiudere la pagina?';
      };
    }
  },
  unmounted: function() {
    window.onbeforeunload = () => undefined;
  },
  methods: {
    sendResults: async function() {
      if (!this.submitBtnInfo().enabled) {
        return;
      }
      this.sending = true
      try {
        await axios.post(process.env.VUE_APP_API_HOST + 'save', this.ratings)
        localStorage.setItem('ratings2021', JSON.stringify(this.ratings))
        window.onbeforeunload = () => undefined;
      } catch (error) {
        alert(`C'è stato un errore nell'invio di dati. Codice errore: ${error.response.data}`);
      }
      this.sending = false
      this.sent = true
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

header {
  width: 80vw;
  max-width: 960px;
  padding: 1em;
  margin: 1em auto 0;
  border-radius: 10px;

  .introduction-text {
    text-align: left;
    padding: 1em;
    li {
      margin: 7px 0;
    }
  }
}
.home {
  // scroll-snap-type: y mandatory;
  // scroll-snap-align: start;

  .presepe-wrapper {
    // scroll-snap-align: start;
    min-height: 80vh;
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
