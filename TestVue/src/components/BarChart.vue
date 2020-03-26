<template>
  <div class="small">
    <bar-chart :chart-data="datacollection"></bar-chart>
    <button @click="refreshData()">刷新</button>
  </div>
</template>

<script>
  import BarChart from './BarChart.js'
  import axios from 'axios'
  axios.defaults.headers.common['Content-Type'] = 'application/json;charset=UTF-8'

  export default {
    components: {
      BarChart
    },
    data () {
      return {
        datacollection: null
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
