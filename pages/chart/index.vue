<template>
  <div class="container">
    <div class="container-head">
      <div class="title">
区块链电子发票数据监控中心
</div>
    </div>
    <div class="container-body">
      <div
class="map-chart" id="map-chart"
style="height: 800px" />
      <div class="other-chart">other</div>
    </div>
  </div>
</template>

<script>
import { init, registerMap } from 'echarts'
import { yunnan } from '../../utils/constant/yunnan'

export default {
  name: '',

  data() {
    return {}
  },

  mounted() {
    this.myChart()
  },

  computed: {},

  methods: {
    myChart() {
      registerMap('yunnan', yunnan)
      // 基于准备好的dom，初始化echarts实例
      const mychart = init(document.getElementById('map-chart'))

      window.addEventListener('resize', function () {
        mychart.resize()
      })

      const data = yunnan.features.map((d) => {
        const { name, center } = d.properties
        return {
          name,
          value: parseInt(Math.random() * 1000),
          coord: center,
        }
      })

      // 绘制图表
      mychart.setOption({
        tooltip: {
          // 触发类型, 数据项图形触发
          trigger: 'item',
          formatter: function (val) {
            // 这里是鼠标滑过显示的弹框，弹框可以根据自己的需求去写
            return val.data.name + val.data.value
          },
        },
        series: [
          {
            type: 'map',
            map: 'yunnan',
            roam: true, // 是否开启鼠标缩放和平移漫游
            geoIndex: 0, // 不可缺少，否则无tooltip 指示效果
            label: {
              normal: {
                show: true, // 显示省份标签
                textStyle: { color: '#fff' }, // 省份标签字体颜色
              },
              emphasis: {
                // 对应的鼠标悬浮效果
                show: true,
                textStyle: { color: '#323232' },
              },
            },
            itemStyle: {
              normal: {
                borderWidth: 0.5, // 区域边框宽度
                borderColor: '#0550c3', // 区域边框颜色
                areaColor: '#032c86', // 区域颜色
              },

              emphasis: {
                borderWidth: 0.95, // 鼠标滑过区域，区域边框宽度
                borderColor: '#fff', // 鼠标滑过区域，区域边框颜色
                areaColor: '#03a9ff', // 鼠标滑过区域背景色
              },
            },
            markPoint: {
              symbol: 'circle',
              symbolSize: 15, // 图片大小
              label: {
                position: 'top',
                color: '#fff',
              },
              // coord数组存放地址坐标
              data,
            },
          },
        ],
      })
    },
  },
}
</script>

<style lang="less">
body {
  background: #012571;
}

.container-head {
  background: #02449b;
  color: #ffffff;
  font-size: 24px;
  font-weight: bold;
  padding: 16px 32px;
}
.container-body {
  display: flex;

  .map-chart {
    width: 50%;
    border-right: 1px solid #1e4e8c;
  }
}
</style>
