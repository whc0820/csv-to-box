<template>
  <v-card class="ma-2">
    <v-card-title>
      <span v-text="title" />
    </v-card-title>

    <v-card-text>
      <div>
        <div class="mx-2 d-flex flex-row justify-start">
          <span class="me-5">95%</span>
          <span v-text="p95" />
        </div>
        <v-divider />
      </div>

      <div>
        <div class="mx-2 d-flex flex-row justify-start">
          <span class="me-5">90%</span>
          <span v-text="p90" />
        </div>
        <v-divider />
      </div>

      <div>
        <div class="mx-2 d-flex flex-row justify-start">
          <span class="me-5">85%</span>
          <span v-text="p85" />
        </div>
        <v-divider />
      </div>

      <div>
        <div class="mx-2 d-flex flex-row justify-start">
          <span class="me-5">80%</span>
          <span v-text="p80" />
        </div>
        <v-divider />
      </div>

      <Plotly :data="data" :layout="layout" :display-mode-bar="false" />
    </v-card-text>
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
      p95: percentile(95, this.source),
      p90: percentile(90, this.source),
      p85: percentile(85, this.source),
      p80: percentile(80, this.source)
    };
  }
};
</script>

<style>
</style>