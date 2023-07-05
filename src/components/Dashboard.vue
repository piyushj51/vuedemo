<script>
  import axios from 'axios';

  export default {
    name: 'ListUsers',
    data() {
      return {
        entries: null,
        sortedbyASC: true,
        tableData: null,
        searchText: ''
      };
    },
    created: function() {
      axios
        .get('https://api.publicapis.org/entries')
        .then(res => {
          this.entries = res.data.entries;
          this.tableData = res.data.entries.slice(0, 9);
        })
    },
    methods: {
      sortData(sortBy) {
        if (this.sortedbyASC) {
          this.tableData.sort((x, y) => (x[sortBy] > y[sortBy] ? -1 : 1));
          this.sortedbyASC = false;
        } else {
          this.tableData.sort((x, y) => (x[sortBy] < y[sortBy] ? -1 : 1));
          this.sortedbyASC = true;
        }
      },
      searchData() {
        const rVal = new RegExp(this.searchText, 'i')
          return this.searchText ? this.tableData.filter((row) =>
            rVal.test(row.API)
          ) : this.tableData;
        },
    },
  }
</script>

<template>
  <div class="container">
    <h2> Entries</h2><input type="text" v-model="searchText" name="search" @onchange="searchData()" placeholder="Search By API Name">
    <table class="table">
      <thead>
        <tr>
          <th class="header-row" title="Sort" scope="col" @click="sortData('API')">Api Name</th>
          <th class="header-row" title="Sort" scope="col" @click="sortData('Auth')">Auth</th>
          <th class="header-row" title="Sort" scope="col" @click="sortData('Category')">Category</th>
          <th class="header-row" title="Sort" scope="col" @click="sortData('Description')">Description</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(data, index) in searchData()" v-bind:key="`${data.API}${index}`"> 
          <td>{{data.API}}</td>
          <td>{{data.Auth}}</td>
          <td>{{data.Category}}</td>
          <td>{{data.Description}}</td>
        </tr>
      </tbody>
    </table> 
  </div> 
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat&display=swap");
input {
  display: block;
  width: 350px;
  margin: 20px auto;
  padding: 10px 45px;
  background-size: 15px 15px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  box-shadow: rgba(50, 50, 93, 0.25) 0px 2px 5px -1px,
    rgba(0, 0, 0, 0.3) 0px 1px 3px -1px;
}
.header-row {
  cursor: pointer;
}
</style>