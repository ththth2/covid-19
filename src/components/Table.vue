<template lang="">
  <div>
    <table class="table table-hover text-center border">
      <thead class="table-dark">
        <tr>
          <th scope="col">Date</th>
          <th scope="col">Cases</th>
          <th scope="col">Deaths</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in tableData.slice().reverse()" :key="index">
          <td scope="row">{{ item.date }}</td>
          <td>{{ item.total }}</td>
          <td>{{ item.deaths }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
<script>
import "bootstrap/dist/css/bootstrap.css";
export default {
  name: "Table",
  props: {
    tableData: {
      type: Array,
    },
  },
  created() {
    const reverseData = this.tableData.reverse();
  },
};
</script>
<style lang=""></style>
