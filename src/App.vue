<template>
  <div id="id" class="container">
    <div class="row mt-5" v-if="arrcases.length > 0">
    <div class="col">
      <h2>Cases</h2>
      <LineChart :chartData="arrcases" :option="chartOptions" label="Cases"></LineChart>
    </div>
  </div>
  </div>
</template>
<script>
import axios from "axios";
import LineChart from "./components/LineChart.vue"
export default {
  name: "App",
  components: {
    LineChart
  },
  data() {
    return {
      arrsum: [],
      arrcases: [],
      arrdeaths: [],
      chartOptions: {
        responsive: true,
        maintainAspectRatio: false
      }
    };
  },
  async created() {
    const { data } = await axios.get(
      "https://disease.sh/v3/covid-19/historical/all?lastdays=30"
    );
    Object.entries(data.cases).forEach(entry  => {
      const [key, value] = entry;
      this.arrcases.push({date: key, total:value});
    });
    Object.values(data.deaths).forEach(val => {
      this.arrdeaths.push({deaths: val});
    });
//  console.log(this.arrdeaths)
    const dates = this.arrcases.map((d) => d.date);
    // const totals = this.arrcases.map((d) => d.total);
    // const deaths = this.arrdeaths.map((d) => d.deaths);
    // this.arrsum.push({date: this.dates, total: this.totals, deaths: this.deaths});
    // console.log(this.arrsum)
    console.log(this.arrcases)
    // console.log(this.arrdeaths)
    let merged = {...this.arrcases, ...this.arrdeaths};
    console.log(merged)


  },
};
</script>

<style scoped></style>
