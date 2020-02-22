<template>
  <v-app>
    <v-app-bar app v-if="headers.length > 0" color="primary" dark>
      <v-tabs v-model="tab" fixed-tabs>
        <v-tab>Source</v-tab>
        <v-tab>Box</v-tab>
      </v-tabs>
    </v-app-bar>
    <v-content>
      <input id="file" type="file" ref="file" style="display: none" @change="onFileChange" />

      <div
        class="pa-5 d-flex flex-column justify-center align-center"
        :style="headers.length == 0 ? 'height:100%':''"
      >
        <v-data-table
          class="ma-2 elevation-2"
          v-if="(headers.length > 0) && (tab == 0)"
          :headers="headers"
          :items="items"
          item-key="n"
        />

        <div
          class="d-flex flex-row flex-wrap justify-center"
          v-if="(headers.length > 0) && (tab == 1)"
        >
          <BoxPlot
            v-for="(item, i) in headers"
            :key="i"
            :title="headers[i].text"
            :source="headers[i].data"
          />
        </div>

        <span class="ma-2" v-if="headers.length == 0" style="font-size:48px;font-weight:300;letter-spacing:0.5em;word-spacing:0.1em;">No Data Yet !</span>
        <v-btn
          class="ma-5"
          v-if="headers.length == 0"
          color="primary"
          text
          @click="$refs.file.click()"
        >
          <span>upload one</span>
        </v-btn>
      </div>
    </v-content>
  </v-app>
</template>

<script>
import BoxPlot from "./components/BoxPlot";

export default {
  name: "App",
  components: {
    BoxPlot
  },
  data: () => ({
    tab: 0,
    rows: [],
    headers: [],
    items: []
  }),
  methods: {
    onFileChange(event) {
      let f = event.target.files[0];
      let fr = new FileReader();
      fr.onload = () => {
        this.headers = [];
        this.items = [];
        this.rows = fr.result.split("\n");
        let cols = this.rows[0].split(",");
        for (let col of cols) {
          this.headers.push({
            text: col,
            value: col,
            data: []
          });
        }

        for (let i in this.rows) {
          if (i != 0) {
            let row = this.rows[i].split(",");

            let item = {
              n: i
            };
            for (let j in cols) {
              item[cols[j]] = row[j];
              this.headers[j].data.push(parseFloat(row[j]));
            }
            this.items.push(item);
          }
        }

        document.getElementById("file").value = "";
      };
      fr.readAsText(f);
    }
  }
};
</script>
