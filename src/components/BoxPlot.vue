<template>
  <v-card class="ma-2">
    <v-card-title>
      <span v-text="title" />
      <v-spacer></v-spacer>
      <v-tooltip left>
        <template v-slot:activator="{ on }">
          <v-btn icon v-on="on" @click="dialog=true">
            <v-icon>mdi-dots-vertical</v-icon>
          </v-btn>
        </template>
        <span>Percentiles</span>
      </v-tooltip>
    </v-card-title>

    <v-card-text>
      <Plotly :data="data" :layout="layout" :display-mode-bar="false" />
    </v-card-text>

    <v-dialog v-model="dialog" width="500" scrollable>
      <v-card>
        <v-card-title>
          <span>{{title}}'s Percentiles</span>
          <v-spacer></v-spacer>
          <v-tooltip left>
            <template v-slot:activator="{ on }">
              <v-btn icon color="red" v-on="on" @click="dialog=false">
                <v-icon>mdi-close</v-icon>
              </v-btn>
            </template>
            <span>Close</span>
          </v-tooltip>
        </v-card-title>
        <v-card-text>
          <v-simple-table dense>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">Percentage</th>
                  <th class="text-left">Percentile</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in percentiles" :key="item.percentage">
                  <td>{{ item.percentage }} %</td>
                  <td>{{ item.percentile }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-card-text>
      </v-card>
    </v-dialog>
  </v-card>
</template>

<script>
import { Plotly } from "vue-plotly";
import percentile from "percentile";

export default {
  components: {
    Plotly
  },
  props: {
    title: String,
    source: Array
  },
  data() {
    return {
      dialog: false,
      data: [
        {
          y: this.source,
          type: "box",
          name: this.title,
          boxmean: "sd",
          boxpoints: "all",
          jitter: 0.3,
          pointpos: -1.8
        }
      ],
      layout: {
        width: 300
      },
      percentiles: []
    };
  },
  created() {
    for (let i = 100; i >= 0; i -= 5) {
      this.percentiles.push({
        percentage: i,
        percentile: percentile(i, this.source)
      });
    }
  }
};
</script>

<style>
</style>