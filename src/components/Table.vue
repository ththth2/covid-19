<template lang="">
  <div>
    <table class="table table-hover text-center border ">
      <thead class="table-dark ">
        <tr>
          <th scope="col">Date</th>
          <th scope="col">Cases</th>
          <th scope="col">Deaths</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item,index) in tableData" :key="index">
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
// const format =  dateFormat(tableData.date, "dd/mm/yyyy");

export default {
  name: "Table",
  props:{
    tableData:{
        type: Array
    },
    arrformat:{
        type: Array
    }
  },
  created() {
    
    // console.log(this.tableData)
    // console.log(arrformat)

  },
};
</script>
<style lang=""></style>
