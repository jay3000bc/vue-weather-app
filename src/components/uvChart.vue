<template>
  <div>
    <div id="chart-container">
      <fusioncharts :type="type" :width="width" :height="height" :dataformat="dataFormat" :dataSource="dataSource">
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

  const chartData = [{
      label: "Venezuela",
      value: "290"
    },
    {
      label: "Saudi",
      value: "260"
    },
    {
      label: "Canada",
      value: "180"
    },
    {
      label: "Iran",
      value: "140"
    },
    {
      label: "Russia",
      value: "115"
    },
    {
      label: "UAE",
      value: "100"
    },
    {
      label: "US",
      value: "30"
    },
    {
      label: "China",
      value: "30"
    }
  ];

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
    props: ["uvi"],
    name: 'app',
    created(){
        
    },
    data() {
      return {
        msg: 'Welcome to Your Vue.js App',
        "type": "angulargauge",
        "renderAt": "chart-container",
        "width": "100%",
        "height": "200",
        "dataFormat": "json",
        dataSource: {
          chart: {
            "caption": this.uvi.toString(),
            "subcaption": "",
            "lowerLimit": "0",
            "upperLimit": "15",
            "theme": "fusion"
          },

          "colorRange": {
            "color": [{
                "minValue": "0",
                "maxValue": "50",
                "code": "#7DA9E0"
              },
              {
                "minValue": "50",
                "maxValue": "75",
                "code": "#D8EDFF"
              },
            ]
          },
          "dials": {
            "dial": [{
              value: this.uvi.toString()
            }]
          }

        },

      }
    },
    methods:{}  
  }

</script>
