<template lang="">
  <div>
    <canvas id="myChart"></canvas>
  </div>
</template>
<script>
import { Line } from "vue-chartjs";
import Chart from 'chart.js/auto'
import 'bootstrap/dist/css/bootstrap.css'
import './css/LineChart.css'
export default {
  extends: Line,
  proos: {
    label: {
      type: String,
    },
    chartData: {
      type: Array,
    },
    options: {
      type: Object,
    },
  },
  mounted() {
    
    const dates = this.chartData.map((d) => d.date);
    const totals = this.chartData.map((d) => d.total);
    
    const ctx = document.getElementById('myChart');

    const labels =  dates
    const data = {
        labels: labels,
        datasets: [{
            label: "Cases",
            data: totals,
            false: false,
            tension: 0.1,
            responsive: true,
            
        }]
    }
    const myChart = new Chart(ctx,{
        type: 'line',
        data: data
    })
    myChart;
  },

};
</script>
<style lang=""></style>
