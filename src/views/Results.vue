<template>
  <div class="result-table-wrapper box">
    <table>
      <tr>
        <th>N°</th>
        <th>Nome</th>
        <th>Media Voti</th>
        <th>Numero voti</th>
      </tr>
      <tr v-for="(v, i) in votes" :key="v.name">
        <td class="cell-number">{{i + 1}}</td>
        <td>{{v.name}}</td>
        <td class="cell-number">{{v.avg}}</td>
        <td class="cell-number">{{v.n}}</td>
      </tr>
    </table>
    <button @click="loadVotes()">Ricarica</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import presepi from "@/assets/presepi.json";
import { PresepeInfo } from "@/models/presepe-info";
import axios from 'axios';

export default defineComponent({
  name: "Results",
  data: function () {
    return {
      presepi: [] as PresepeInfo[],
      votes: [] as { name: string; avg: number; n: number}[]
    };
  },
  components: {
  },
  created: async function () {
    this.$data.presepi = presepi;
    this.loadVotes()
  },
  methods: {
    loadVotes: async function() {
      let response: { [id: string]: { avg: number; n: number} } = { }
      try {
        response = (await axios.get(process.env.VUE_APP_API_HOST + 'result')).data
      } catch (error) {
          alert('Errore caricamento dati');
          return;
      }
      const votes: { name: string; avg: number; n: number}[] = []
      for (const k in response) {
        votes.push({
          name: presepi.find(p => p.code === k)?.name ?? k,
          n: response[k].n,
          avg: Math.round(response[k].avg * 100) / 100
        })
      }
      this.votes = votes.sort((a, b) => b.avg - a.avg)
    }
  }
});
</script>

<style lang="scss">
.result-table-wrapper {
  margin: 1em;
  padding: 1em;
  display: inline-block;
}
table {
  text-align: left;
  color: white;
  margin: 1em auto;
  .cell-number {
    text-align: right;
  }
  th, td {
    border: 1px solid white;
    border-collapse: collapse;
    padding: 5px;
  }
}
button {
  font-size: 1em;
}
</style>