<template lang="">
  <div class="row d-flex justify-content-center">
    <div class="pie">
      <div class="row">
        <div class="col-12 py-2"><canvas id="myChart3"></canvas></div>
      </div>
    </div>
    <div class="row">
      <div class="col-sm-12 col-lg-6 h-50 py-2">
        <canvas id="myChart"></canvas>
      </div>
      <div class="col-sm-12 col-lg-6 h-50 py-2">
        <canvas id="myChart2"></canvas>
      </div>
    </div>
  </div>
</template>
<script>
import { Line } from "vue-chartjs";
import Chart from "chart.js/auto";
import "bootstrap/dist/css/bootstrap.css";
import "./css/LineChart.css";
import axios from "axios";

export default {
  extends: Line,
  proos: {
    label: {
      type: String,
    },
    chartData: {
      type: Array,
    },
    chartData2: {
      type: Object,
    },
    options: {
      type: Object,
    },
  },
  created() {},
  async mounted() {
    const requestTwo = await axios.get("https://disease.sh/v3/covid-19/all");
    const datasum = requestTwo.data;
    const casessum = datasum["cases"];
    const deathssum = datasum["deaths"];
    const recoveredsum = datasum["recovered"];

    const dates = this.chartData.map((d) => d.date);
    const totals = this.chartData.map((d) => d.total);
    const deaths = this.chartData.map((d) => d.deaths);

    const labels = dates;
    const data = {
      labels: labels,
      datasets: [
        {
          label: "Cases",
          data: totals,
          false: false,
          tension: 0.1,
          responsive: true,
          backgroundColor: "#FFF66E",
          borderColor: "rgb(255, 205, 86)",
        },
      ],
    };
    const data2 = {
      labels: labels,
      datasets: [
        {
          label: "Deaths",
          data: deaths,
          false: false,
          tension: 0.1,
          backgroundColor: "#F75F69",
          borderColor: "#AF0000",
        },
      ],
    };
    const data3 = {
      labels: ["Cases", "Deaths", "Recovered"],
      datasets: [
        {
          label: "Cases",
          data: [casessum, deathssum, recoveredsum],
          fill: false,
          backgroundColor: ["#FFF66E", "#F75F69", "#27DE64"],
          borderColor: ["rgb(255, 205, 86)", "#AF0000", "#268E37"],
          tension: 0.1,
        },
      ],
    };
    const myChart = new Chart(document.getElementById("myChart"), {
      type: "line",
      data: data,
    });
    myChart;
    const myChart2 = new Chart(document.getElementById("myChart2"), {
      type: "line",
      data: data2,
    });
    myChart2;
    const myChart3 = new Chart(document.getElementById("myChart3"), {
      type: "pie",
      data: data3,
    });
    myChart2;
  },
};
</script>
<style lang=""></style>
