<template>
  <Navbar/>
  <div id="id" class="container">
    <div class="row mt-5" v-if="arrsum.length > 0">
      <div class="col">
        <LineChart
          :chartData2="arrcases"
          :chartData="arrformat"
          :option="chartOptions"
        ></LineChart>
        <div class="mt-4">
          <Table :tableData="arrformat" />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import "bootstrap/dist/css/bootstrap.css";
import axios from "axios";
import LineChart from "./components/LineChartCases.vue";
import Table from "./components/Table.vue";
import dateFormat from "dateformat";
import Navbar from "./components/Navbar.vue"

export default {
  name: "App",
  components: {
    LineChart,
    Table,
    Navbar
  },
  data() {
    return {
      arrsum: [],
      arrcases: [],
      arrdeaths: [],
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
    const dates = this.arrcases.map((d) => d.date);
    const totals = this.arrcases.map((d) => d.total);
    this.arrsum = this.arrcases.map((item, i) =>
      Object.assign({}, item, this.arrdeaths[i])
    );
    const arrformat = [];
    Object.entries(this.arrsum).forEach((entry) => {
      const [key, value] = entry;
      const format = dateFormat(value.date, "dd/mm/yyyy");
      this.arrformat.push({
        date: format,
        total: value.total,
        deaths: value.deaths,
      });
    });
    this.arrformat.reverse();
  },
};
</script>

<style scoped></style>
