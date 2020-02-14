<template>
  <div class="hello">
    <!-- 初始化 echarts 需要准备容器 -->
    <div ref="container" style="height: 500px; width: 700px"></div>
  </div>
</template>

<script>
import jsonp from 'jsonp'
// ECharts 中提供了两种格式的地图数据，一种是可以直接 script 标签引入的 js 文件，
// 引入后会自动注册地图名字和数据。还有一种是 JSON 文件，需要通过 AJAX 异步加载后手动注册。
import echarts from 'echarts'
import 'echarts/map/js/china.js'

const option = {
  title: {
    text: '疫情地图', // 主标题文本
    subtext: '数据来源：国家及各省市地区卫健委', // 副标题文本
  },
  series: [
    {
      name: '确诊人数', // 系列名称，用于 tooltip 的显示
      type: 'map', // 渲染类型为地图
      map: 'china', // 渲染中国地图
      roam: 'scale', // 开启缩放
      // 图形上的文本标签（地区名称）
      label: {
        show: true,
        color: '#333',
        fontSize: 10,
      },
      // 高亮状态下的多边形和标签样式
      emphasis: {
        label: {
          color: '#fff',
          fontSize: 12,
        },
        itemStyle: {
          areaColor: 'blue', // 地图区域的颜色
        },
      },
      data: [], // 数据
    },
  ],
  // 视觉映射组件，用于进行『视觉编码』，也就是将数据映射到视觉元素（视觉通道）
  visualMap: [
    {
      type: 'piecewise', // 分段型视觉映射
      show: true,
      // 分段
      pieces: [
        { min: 10000 },
        { min: 1000, max: 9999 },
        { min: 100, max: 999 },
        { min: 10, max: 99 },
        { min: 1, max: 9 },
        { min: 0, max: 0 },
      ],
      // 定义 在选中范围中 的视觉元素
      inRange: {
        symbol: 'rect', // 图元的图形类别
        // 图元的颜色
        color: [
          '#f5f5f5',
          '#f6c2b4',
          '#da9589',
          '#c1695f',
          '#a73e36',
          '#8f1d14',
        ],
      },
    },
  ],
  // 提示框组件
  tooltip: {
    trigger: 'item', // 数据项图形触发
  },
  // 工具栏
  toolbox: {
    show: true,
    orient: 'verticle',
    left: 'right',
    top: 'center',
    feature: {
      saveAsImage: {}, // 保存为图片
    },
  },
}

export default {
  name: 'MyMap',
  mounted() {
    this.getData()
    // 步骤1：初始化
    this.myChart = echarts.init(this.$refs.container)
    // 步骤2：传配置（数据）
    this.myChart.setOption(option)
  },
  methods: {
    getData() {
      // jsonp(url, opts, fn)
      jsonp(
        'https://interface.sina.cn/news/wap/fymap2020_data.d.json?1580892522427',
        {},
        (err, data) => {
          if (!err) {
            const list = data.data.list.map(item => ({
              name: item.name,
              value: item.value,
            }))
            option.series[0].data = list
            // 执行前提是，DOM 渲染完成
            this.myChart.setOption(option)
          }
        }
      )
    },
  },
}
</script>
