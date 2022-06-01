<template>
    <div>
    <div id="chart-container">
      <fusioncharts
      :type="type"
      :width="width"
      :height="height"
      :dataformat="dataFormat"
      :dataSource="dataSource"
      >
      </fusioncharts>
    </div>
    </div>
</template>

<script>

import Vue from 'vue';
import VueFusionCharts from 'vue-fusioncharts';
import FusionCharts from 'fusioncharts';
import Column2D from 'fusioncharts/fusioncharts.charts';
import FusionTheme from 'fusioncharts/themes/fusioncharts.theme.fusion';

Vue.use(VueFusionCharts, FusionCharts, Column2D, FusionTheme);

// const chartData = [
//     {
//       label: "Venezuela",
//       value: "290"
//     },
//     {
//       label: "Saudi",
//       value: "260"
//     },
//     {
//       label: "Canada",
//       value: "180"
//     },
//     {
//       label: "Iran",
//       value: "140"
//     },
//     {
//       label: "Russia",
//       value: "115"
//     },
//     {
//       label: "UAE",
//       value: "100"
//     },
//     {
//       label: "US",
//       value: "30"
//     },
//     {
//       label: "China",
//       value: "30"
//     }
//   ];

// const dataSource = {
//   chart: {
//     caption: "Countries With Most Oil Reserves [2017-18]",
//     subcaption: "In MMbbl = One Million barrels",
//     xaxisname: "Country",
//     yaxisname: "Reserves (MMbbl)",
//     numbersuffix: "K",
//     theme: "fusion"
//   },
//   data: chartData
//   };


export default {
  props: ["chartData"],
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      "type": "spline",
      "renderAt": "chart-container",
      // "width": "550",
      // "height": "350",
      "width": "100%",
      "height": "309",
      "dataFormat": "json",
      // dataSource
 
      dataSource :{
      chart: {
        caption: "",
        subcaption: "",
        xaxisname: "Time",
        yaxisname: "Temerature",
        numbersuffix: "",
        theme: "fusion"
      },
      data: this.chartData
      },

    }
  }
}

</script>
