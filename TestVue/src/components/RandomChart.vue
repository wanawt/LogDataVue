<template>
  <div class="small">
    <div class="hhrow1">
      <line-chart :chart-data="datacollection"></line-chart>
     <el-date-picker
              v-model="value1"
              type="date"
              format="yyyy-MM-dd"
              value-format="yyyy-MM-dd"
              placeholder="选择日期时间">
      </el-date-picker>
      <el-button type="primary" @click="clickBtn()">确定</el-button>
    </div>
    <div class="hhrow">
      <bar-chart :chart-data="bardatacollection"></bar-chart>
      <el-date-picker
              v-model="value2"
              type="date"
              format="yyyy-MM-dd"
              value-format="yyyy-MM-dd"
              placeholder="选择日期时间">
      </el-date-picker>
      <el-button type="primary" @click="refreshData()">确定</el-button>
    </div><div class="hhrow">
      <bar-chart :chart-data="visitDataCollection"></bar-chart>
      <el-date-picker
              v-model="value3"
              type="date"
              format="yyyy-MM-dd"
              value-format="yyyy-MM-dd"
              placeholder="选择日期时间">
      </el-date-picker>
      <el-button type="primary" @click="refreshVisitData()">确定</el-button>
    </div><div class="hhrow">
      <pie-chart :chart-data="piedatacollection"></pie-chart>
      <el-button type="primary" @click="refreshPieData()">刷新</el-button>
    </div>
  </div>
</template>

<script>
  import LineChart from './LineChart.js'
  import BarChart from './BarChart.js'
  import PieChart from './PieChart.js'
  import axios from 'axios'
  axios.defaults.headers.common['Content-Type'] = 'application/json;charset=UTF-8'

  export default {
    components: {
      LineChart,
      BarChart,
      PieChart
    },
    data () {
      return {
        datacollection: null,
        bardatacollection: null,
        piedatacollection: null,
        visitDataCollection: null,
        value1: "",
        value2: "",
        value3: ""
      }
    },
    mounted () {
      // this.refreshData(),
      this.refreshPieData()
      // this.refreshVisitData()
    },

    methods: {
      // 日活
      clickBtn() {
          var titleList = [];
          var countList = [];
          axios.post("http://localhost:8080/queryActiveList",{
          qDate:this.value1}).then(
            response=> {
              for (var i=0; i<response.data.length; i++) {
                console.log(response.data[i]);
                var ite = response.data[i];
                countList.push(ite['count']);
                titleList.push(ite['log_date']);
              }
              this.datacollection = {
                labels: titleList,
                datasets: [
                  {
                    label: '日活',
                    backgroundColor: "rgba(75,192,192,0.4)",
                    data: countList
                  }
                ]
              }
            }
          );
      },

      // 访问最多商品
      refreshVisitData() {
        var titleList = [];
        var countList = [];

        axios.post("http://localhost:8080/queryVisitList",{
        qDate:this.value3}).then(
          response=> {
            for (var i=0; i<response.data.length; i++) {
              console.log(response.data[i]);
              var ite = response.data[i];
              countList.push(ite['count']);
              titleList.push(ite['title']);
            }
            this.visitDataCollection = {
              labels: titleList,
              datasets: [
                {
                  label: this.value3+'至今访问最多商品',
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
          });
      },

      // 设备分布
      refreshPieData() {
          var titleList = [];
          var countList = [];
          axios.get("http://localhost:8080/queryOSList").then(
            response=> {
              for (var i=0; i<response.data.length; i++) {
                console.log(response.data[i]);
                var ite = response.data[i];
                countList.push(ite['count']);
                titleList.push(ite['os']);
              }
              this.piedatacollection = {
                labels: titleList,
                datasets: [
                  {
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
                                        'rgba(255, 99, 132, 0.2)',
                                        'rgba(54, 162, 235, 0.2)',
                                        'rgba(255, 206, 86, 0.2)',
                                        'rgba(75, 192, 192, 0.2)',
                                        'rgba(153, 102, 255, 0.2)',
                                        'rgba(255, 159, 64, 0.2)'
                                    ],
                  }
                ]
              }
            }
          );
      },

      // 搜索排行
      refreshData() {
        var titleList = [];
        var countList = [];

        axios.post("http://localhost:8080/queryLogData",{
        qDate:this.value2}).then(
          response=> {
            for (var i=0; i<response.data.length; i++) {
              console.log(response.data[i]);
              var ite = response.data[i];
              countList.push(ite['count']);
              titleList.push(ite['keyword']);
            }
            this.bardatacollection = {
              labels: titleList,
              datasets: [
                {
                  label: this.value2+'至今搜索前五',
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
          });
      }
    }
  }
</script>

<style>
  .small {
    max-width: 1200px;
    margin:  20px auto;
  }
  .hhrow1 {
    width: 65%;
    display:inline-block;
  }
  .hhrow {
    width: 36%;
    display:inline-block;
  }
</style>
