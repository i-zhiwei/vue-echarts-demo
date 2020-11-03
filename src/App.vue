<template>
  <div id="app" class="main">
    <div id="chartLine" class="card"></div>
    <div id="chartMap" class="card"></div>
  </div>
</template>

<script>
import kunshanJson from "./assets/map/kunshan.json";
export default {
  data() {
    return {
      jam: "hello jam!",
    };
  },
  mounted() {
    this.drawChart();
    this.drawMap();
  },
  methods: {
    drawChart() {
      let myChart = this.$echarts.init(document.getElementById("chartLine"));
      myChart.setOption({
        title: {
          text: "ECharts 入门示例",
        },
        tooltip: {},
        xAxis: {
          data: ["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"],
        },
        yAxis: {},
        series: [
          {
            name: "销量",
            type: "bar",
            data: [5, 20, 36, 10, 10, 20],
          },
        ],
      });
    },
    drawMap() {
      // 注册地图
      this.$echarts.registerMap("kunshanJson", kunshanJson);
      let myMap = this.$echarts.init(document.getElementById("chartMap"));

      let data = [
        { name: "周庄镇", value: 1, level: 2 },
        { name: "花桥镇", value: 2, level: 2 },
        { name: "周市镇", value: 3, level: 1 },
        { name: "巴城镇", value: 4, level: 1 },
        { name: "玉山镇", value: 5, level: 2 },
        { name: "开发区", value: 6, level: 3 },
        { name: "锦溪镇", value: 7, level: 2 },
        { name: "淀山湖镇", value: 8, level: 3 },
        { name: "张浦镇", value: 9, level: 1 },
        { name: "千灯镇", value: 10, level: 2 },
        { name: "陆家镇", value: 11, level: 3 },
      ];
      let geoCoordMap = {
        周庄镇: [120.8448, 31.1435],
        花桥镇: [121.1119, 31.2973],
        周市镇: [120.9835, 31.4738],
        巴城镇: [120.8551, 31.4609],
        玉山镇: [120.9409, 31.4046],
        开发区: [121.0426, 31.3788],
        锦溪镇: [120.9169, 31.1811],
        淀山湖镇: [121.0226, 31.1729],
        张浦镇: [120.9409, 31.3038],
        千灯镇: [121.0165, 31.2551],
        陆家镇: [121.0453, 31.3237],
      };
      let levelColorMap = {
        1: "rgba(100,214,207,1)",
        2: "rgba(255, 227, 123, 1)",
        3: "rgba(218, 137, 126, 1)",
      };
      function initSeriesData() {
        var temp = [];
        for (var i = 0; i < data.length; i++) {
          var geoCoord = geoCoordMap[data[i].name];
          if (geoCoord) {
            temp.push({
              name: data[i].name,
              value: geoCoord.concat(data[i].value, data[i].level),
            });
          }
        }
        return temp;
      }

      myMap.setOption({
        tooltip: {
          trigger: "item",
          formatter: function (params) {
            return params.name;
          },
        },
        legend: {
          orient: "vertical",
          id: 1,
          y: "bottom",
          x: "right",
          itemWidth: 15,
          data: [],
          textStyle: {
            color: "#fff",
          },
        },
        geo: {
          show: true,
          map: "kunshanJson",
          label: {
            normal: {
              show: false,
            },
            emphasis: {
              show: false,
            },
          },
          zoom: 1.2,
          roam: true,
          itemStyle: {
            normal: {
              areaColor: "transparent",
              borderColor: "rgba(106,205,244,1)",
              borderWidth: 2,
              shadowColor: "rgba(106,205,244,.5)",
              shadowBlur: 30,
            },
            emphasis: {
              areaColor: "#0455a7",
            },
          },
        },
        series: [
          {
            type: "effectScatter",
            coordinateSystem: "geo",
            showEffectOn: "render",
            symbolSize: 10,
            rippleEffect: {
              period: 8,
              scale: 3,
              brushType: "fill",
            },
            hoverAnimation: true,
            label: {
              normal: {
                formatter: "{b}",
                position: "top",
                show: true,
              },
            },
            itemStyle: {
              normal: {
                color: function (params) {
                  return levelColorMap[params.value[3]];
                },
                shadowBlur: 10,
                shadowColor: "#333",
              },
            },
            zlevel: 1,
            data: initSeriesData(data),
          },
        ],
      });
      // 添加事件
      myMap.on("click", (params) => {
        console.log(params);
        alert(`${params.name}`);
      });
    },
  },
};
</script>

<style>
div {
  box-sizing: border-box;
}
.main {
  display: flex;
  flex-flow: column wrap;
  justify-content: center;
  align-items: center;
}
.main .card {
  width: 500px;
  height: 500px;
  border: 1px solid red;
  margin-bottom: 100px;
}
</style>
