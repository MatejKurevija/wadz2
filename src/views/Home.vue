<template>
  <v-container>
        <v-form>
            <div class="center">
              <v-text-field outlined label="unesite ime" v-model="ui"></v-text-field>
            </div>

            <v-btn color="green" class="button" @click="dohvatipodatke()">pretrazi</v-btn>
         </v-form>

         <v-data-table
            :headers="headers"
            :items="pIme"
            hide-default-footer
            class="elevation-1"
            style="margin-top: 5rem">
        </v-data-table>
  </v-container>
</template>

<script>
import axios from "axios";


export default {
  name: "Home",
  data() {
    return {
      headers: [
        {
          text: "Ime",
          align: "start",
          sortable: false,
          value: "ime",
        },
        {
          text: "Godina",
          value: "godinaKor",
        },
        {
          text: "Spol",
          value: "spolKor",
        },
        {
          text: "Nacionalnost",
          value: "nacKor",
        },
      ],

      pIme: [],

      godina: "",
      spol: "",
      nac: "",
      ui: "",
    };
  },

  methods: {
    async dohvatipodatke() {
      const godData = await axios.get(
        "https://api.agify.io?name=" + this.ui
      );
      this.godina = godData.data;
      const spolData = await axios.get(
        "https://api.genderize.io?name=" + this.ui
      );
      this.spol = spolData.data;
      const nacData = await axios.get(
        "https://api.nationalize.io?name=" + this.ui
      );
      this.nac = nacData.data;

      this.pIme.push({
        ime: this.ui,
        godinaKor: this.godina.age,
        nacKor:
          this.nac.country[1].country_id +
          " (" +
          (this.nac.country[1].probability*100).toFixed(0) + "%)",
        spolKor: this.spol.gender +
          " (" +
          (this.spol.probability*100).toFixed(0) + "%)",
      });
    },
  },
};
</script>

<style scoped>

.center {
  display: flex;
  margin: 0 auto;
  width: 40%;
  margin-top: 2rem;
}
.button {
  display: flex;
  justify-content: center;
  margin: 0 auto;
  margin-top: 1rem;
}
</style>