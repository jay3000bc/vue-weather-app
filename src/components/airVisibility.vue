<template>
  <div>
    <div id="chart-container1">
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
  export default {
    props: ["visibility"],
    name: 'app',
    data() {
      return {

        msg: 'Welcome to Your Vue.js App',
        "type": "hlineargauge",
        "renderAt": "chart-container1",
        "width": "100%",
        "height": "200",
        "dataFormat": "json",
        // dataSource

        dataSource: {
          chart: {
            caption: "Air Visibility",
            captionFontBold: "0",
            captionFontColor: "#000000",
            baseFont: "Roboto",
            numberSuffix: " km",
            lowerLimit: "0",
            upperLimit: "40",
            showPointerShadow: "1",
            animation: "1",
            transposeAnimation: "1",
            theme: "fusion",
            bgAlpha: "0",
            canvasBgAlpha: "0",
            valueFontSize: "20",
            valueFontColor: "#000000",
            valueFontBold: "1",
            pointerBorderAlpha: "0",
            chartBottomMargin: "40",
            captionPadding: "30",
            chartTopMargin: "30"
          },
          colorRange: {
            color: [{
                minValue: "0",
                maxValue: "4",
                label: "Fog",
                code: "#6297d9"
              },
              {
                minValue: "4",
                maxValue: "10",
                label: "Haze",
                code: "#7DA9E0"
              },
              {
                minValue: "10",
                maxValue: "40",
                label: "Clear",
                code: "#D8EDFF"
              }
            ]
          },
          pointers: {
            pointer: [{
              value: this.visibility.toString()
            }]
          }





        },

      }
    }
  }

</script>
