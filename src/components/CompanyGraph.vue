<template>
  <v-chart class="chart" :option="option" />
</template>

<script>
import { use } from 'echarts/core'
import { CanvasRenderer } from 'echarts/renderers'
import { GraphChart } from 'echarts/charts'
import {
  TitleComponent,
  TooltipComponent,
  LegendComponent
} from 'echarts/components'
import VChart, { THEME_KEY } from 'vue-echarts'
import { ref, defineComponent } from 'vue'
import sampleData from '@/assets/data/sample.json'

use([
  CanvasRenderer,
  GraphChart,
  TitleComponent,
  TooltipComponent,
  LegendComponent
])

// const sampleData = await fetch('/data/sample.json').catch()

console.log(sampleData)

export default defineComponent({
  name: 'CompanyGraph',
  components: {
    VChart
  },
  provide: {
    [THEME_KEY]: 'dark'
  },
  data () {
    return {
      nodes: [{
        name: 'Node 1'
      }, {
        name: 'Node 2',
        x: 200,
        y: 200
      }, {
        name: 'Node 3',
        x: 300,
        y: 300
      }],
      edges: [{
        source: 0,
        target: 1
      }, {
        source: 1,
        target: 2
      }]
    }
  },
  setup () {
    console.log(this)
    const option = ref({
      title: {
        text: 'Les Miserables',
        subtext: 'Default layout',
        top: 'top',
        left: 'right'
      },
      tooltip: {},
      // legend: [{
      //   // selectedMode: 'single',
      //   data: sampleData.categories.map(function (a) {
      //     return a.name
      //   })
      // }],
      animationDuration: 1500,
      animationEasingUpdate: 'quinticInOut',
      series: [
        {
          name: 'sample',
          type: 'graph',
          layout: 'force',
          force: { // 力引导布局相关的配置项
            repulsion: 80, // 节点之间的斥力因子
            gravity: 0.02, // 节点受到的向中心的引力因子 越大越往中心靠拢
            edgeLength: 240, // 边的两个节点之间的距离
            layoutAnimation: false // 显示布局的迭代动画
          },
          edgeSymbol: ['none', 'arrow'],
          data: [{
            name: 'Node 1'
          }, {
            name: 'Node 2'
          }, {
            name: 'Node 3'
          }],
          links: [{
            source: 0,
            target: 1
          }, {
            source: 1,
            target: 2
          }],
          // categories: sampleData.categories,
          roam: true,
          label: {
            position: 'right',
            formatter: '{b}'
          },
          lineStyle: {
            color: 'source',
            curveness: 0.3
          },
          emphasis: {
            focus: 'adjacency',
            lineStyle: {
              width: 10
            }
          }
        }
      ]
    })

    return { option }
  }
})
</script>

<style scoped>
.chart {
  height: 100vh;
}
</style>
