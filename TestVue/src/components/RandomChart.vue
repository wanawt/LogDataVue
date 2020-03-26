<template>
  <div class="small">
    <div>
      <line-chart :chart-data="datacollection"></line-chart>
      <button @click="refreshData()">刷新</button>
    </div>
    <div>
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
      this.fillData()
    },
    methods: {
      fillData () {
        this.datacollection = {
          labels: ['12日', '13日', '14日', '15日', '16日'],
          datasets: [
            {
              label: '商品浏览量',
              backgroundColor: "rgba(75,192,192,0.4)",
              data: [this.getRandomInt(), this.getRandomInt(), this.getRandomInt(), this.getRandomInt(), this.getRandomInt()]
            }
          ]
        }
        this.bardatacollection = {
          labels: ['12日', '13日', '14日', '15日', '16日'],
          datasets: [
            {
              label: '商品浏览量',
              backgroundColor: "rgba(75,192,192,0.4)",
              data: [this.getRandomInt(), this.getRandomInt(), this.getRandomInt(), this.getRandomInt(), this.getRandomInt()]
            }
          ]
        }
      },
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
                  label: '搜索前五',
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
    max-width: 600px;
    margin:  150px auto;
  }
</style>
