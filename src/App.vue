<template>
  <div id="id" class="container">
    <div class="row mt-5" v-if="arrsum.length > 0">
      <div class="col">
        <h2>Cases</h2>
        <LineChart
          :chartData2="arrcases"
          :chartData="arrformat"
          :option="chartOptions"
          label="Cases"
        ></LineChart>
        <div class="mt-4">
          <Table :tableData="arrformat"/>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import "./components/css/App.css";
import "bootstrap/dist/css/bootstrap.css";
import axios from "axios";
import LineChart from "./components/LineChartCases.vue";
import Table from "./components/Table.vue"
import dateFormat, { masks } from "dateformat";


export default {
  name: "App",
  components: {
    LineChart,
    Table
  },
  data() {
    return {
      arrsum: [],
      arrcases: [],
      arrdeaths: [],
      arrdatasum: [],
      arrformat: [],
      chartOptions: {
        responsive: false,
        maintainAspectRatio: false,
      },
    };
  },
  async created() {
    const { data } = await axios.get(
      "https://disease.sh/v3/covid-19/historical/all?lastdays=30"
    );
    Object.entries(data.cases).forEach((entry) => {
      const [key, value] = entry;
      this.arrcases.push({ date: key, total: value });
    });
    Object.entries(data.deaths).forEach((entry) => {
      const [key, value] = entry;
      this.arrdeaths.push({ date: key, deaths: value });
    });
    //  console.log(this.arrdeaths)
    const dates = this.arrcases.map((d) => d.date);
    const totals = this.arrcases.map((d) => d.total);
    // const deaths = this.arrdeaths.map((d) => d.deaths);
    // this.arrsum.push({date: this.dates, total: this.totals, deaths: this.deaths});
    // console.log(dates)
    // console.log(this.arrdeaths);
    // console.log(this.arrdeaths);
    
    // document.body.innerHTML = JSON.stringify(this.arrdeaths);
    this.arrsum = this.arrcases.map((item, i) =>
      Object.assign({}, item, this.arrdeaths[i])
    );
    const arrformat = []
    Object.entries(this.arrsum).forEach((entry) => {
      const [key, value] = entry;
      const format =  dateFormat(value.date, "dd/mm/yyyy");
      this.arrformat.push({date: format ,total: value.total, deaths: value.deaths});

    });
    this.arrformat.reverse();
    console.log(this.arrformat)
    
    // let merged = {...this.arrsum, ...this.datasum};
    // console.log(this.arrsum)
  },
};
</script>

<style scoped></style>
