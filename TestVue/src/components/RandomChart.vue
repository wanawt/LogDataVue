<template>
  <div class="small">
    <div class="hhrow">
      <line-chart :chart-data="datacollection"></line-chart>
      <button @click="refreshData()">刷新</button>
    </div><div class="hhrow">
      <bar-chart :chart-data="bardatacollection"></bar-chart>
      <button @click="refreshData()">刷新</button>
    </div>
  </div>
</template>

<script>
  import LineChart from './LineChart.js'
  import BarChart from './BarChart.js'
  import axios from 'axios'
  axios.defaults.headers.common['Content-Type'] = 'application/json;charset=UTF-8'

  export default {
    components: {
      LineChart,
      BarChart
    },
    data () {
      return {
        datacollection: null,
        bardatacollection: null
      }
    },
    mounted () {
      this.refreshData()
    },
    methods: {
      refreshData() {
        var titleList = [];
        var countList = [];
        axios.get("http://localhost:8080/queryLogData").then(
          response=> {
            for (var i=0; i<response.data.length; i++) {
              console.log(response.data[i]);
              var ite = response.data[i];
              countList.push(ite['count']);
              titleList.push(ite['keyword']);
            }
            this.datacollection = {
              labels: titleList,
              datasets: [
                {
                  label: '搜索前五',
                  backgroundColor: "rgba(75,192,192,0.4)",
                  data: countList
                }
              ]
            }
            this.bardatacollection = {
              labels: titleList,
              datasets: [
                {
                  label: '3月25日搜索前五',
                  backgroundColor: "rgba(75,192,192,0.4)",
                  data: countList,
                  backgroundColor: [
                                      'rgba(255, 99, 132, 0.2)',
                                      'rgba(54, 162, 235, 0.2)',
                                      'rgba(255, 206, 86, 0.2)',
                                      'rgba(75, 192, 192, 0.2)',
                                      'rgba(153, 102, 255, 0.2)',
                                      'rgba(255, 159, 64, 0.2)'
                                  ],
                  borderColor: [
                                      'rgba(255,99,132,1)',
                                      'rgba(54, 162, 235, 1)',
                                      'rgba(255, 206, 86, 1)',
                                      'rgba(75, 192, 192, 1)',
                                      'rgba(153, 102, 255, 1)',
                                      'rgba(255, 159, 64, 1)'
                                  ],
                }
              ]
            }
          }
        );

      },
      getRandomInt () {
        return Math.floor(Math.random() * (50 - 5 + 1)) + 15
      }
    }
  }
</script>

<style>
  .small {
    max-width: 1000px;
    margin:  20px auto;
  }
  .hhrow {
    width: 50%;
    display:inline-block;
  }
</style>
