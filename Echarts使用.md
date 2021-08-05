## Echarts使用

#### legend 图例

#### xAxis 

#### yAxis



toolbox工具栏



#### dataZoom 是否可缩放

##### type

​	slider 拉着滑动

​	inside 鼠标滚动



#### tooltip

##### formatter

params的相关属性与值

![](C:\Users\刘颖\Documents\tips截图\【echarts】tooltip_formatter_params.png)





#### grid

配置直角坐标系的位置，可数组形式，配置多个

#### dataset 管理数据



类目轴，默认情况下，类目轴对应到dataset第一列

数值轴



series.encode.tooltip设置dimension，会在option下设置tooltip以后显示



#### transform 进行数据转换

在 echarts 中，“数据转换” 这个词指的是，给定一个已有的“数据集”（[dataset](https://echarts.apache.org/zh/option.html#dataset)）和一个“转换方法”（[transform](https://echarts.apache.org/zh/option.html#dataset.transform)），echarts 能生成一个新的“数据集”，然后可以使用这个新的“数据集”绘制图表。这些工作都可以声明式地完成。抽象地来说，数据转换是这样一种公式：`outData = f(inputData)`。`f` 是转换方法，例如：`filter`、`sort`、`regression`、`boxplot`、`cluster`、`aggregate`(todo) 等等。

##### 可完成

- 把数据分成多份用不同的饼图展现。
- 进行一些数据统计运算，并展示结果。
- 用某些数据可视化算法处理数据，并展示结果。
- 数据排序。
- 去除或直选择数据项





#### emphasis Echarts中的样式

版本4与5有区别

在鼠标悬浮到图形元素上时，一般会出现高亮的样式。默认情况下，高亮的样式是根据普通样式自动生成的。但是高亮的样式也可以自己定义，主要是通过 [emphasis](https://echarts.apache.org/zh/option.html#series-scatter.emphasis) 属性来定制。[emphsis](https://echarts.apache.org/zh/option.html#series-scatter.emphasis) 中的结构，和普通样式的结构相同，例如：

```js
option = {
    series: {
        type: 'scatter',

        // 普通样式。
        itemStyle: {
            // 点的颜色。
            color: 'red'
        },
        label: {
            show: true,
            // 标签的文字。
            formatter: 'This is a normal label.'
        },

        // 高亮样式。
        emphasis: {
            itemStyle: {
                // 高亮时点的颜色。
                color: 'blue'
            },
            label: {
                show: true,
                // 高亮时标签的文字。
                formatter: 'This is a emphasis label.'
            }
        }
    }
}
```

注意：在 ECharts4 以前，高亮和普通样式的写法，是这样的：

```js
option = {
    series: {
        type: 'scatter',

        itemStyle: {
            // 普通样式。
            normal: {
                // 点的颜色。
                color: 'red'
            },
            // 高亮样式。
            emphasis: {
                // 高亮时点的颜色。
                color: 'blue'
            }
        },

        label: {
            // 普通样式。
            normal: {
                show: true,
                // 标签的文字。
                formatter: 'This is a normal label.'
            },
            // 高亮样式。
            emphasis: {
                show: true,
                // 高亮时标签的文字。
                formatter: 'This is a emphasis label.'
            }
        }
    }
}
```

这种写法 **仍然被兼容**，但是，不再推荐。事实上，多数情况下，使用者只会配置普通状态下的样式，而使用默认的高亮样式。所以在 ECharts4 中，支持不写 `normal` 的配置方法（即本文开头的那种写法），使得配置项更扁平简单。



#### 异步加载和更新





### Vue项目中使用Echarts

```js
import echarts from 'echarts'

//监听器      
watch: {
    data  () {
      this.drawpie()
    }
  },
//挂载
  mounted () {
    this.drawpie()
  },


//methods中

// 饼状图
    drawpie () {
      var myChart = echarts.init(window.document.getElementById('echarts'))
      myChart.setOption({
        series: [
          {
            type: 'pie',
            radius: '55%',
            center: ['40%', '40%'],
            data: [
              {value: this.noJoin, name: '未参与'},
              {value: this.join, name: '已参与'}
            ],
            emphasis: {
              itemStyle: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            },
            color: ['red', 'green']
          }
        ]
      })
    },
        
    
        
<div id="echarts"></div>
        
#echarts {
    width: 600PX;
    height: 100%;
    display: inline-block;
  }
```





根据异步获取的内容渲染Echarts图表

```js
import chinaCities from 'echarts/map/json/china-cities.json'

let geoCoordMap = {}
chinaCities.features.map(item => {
  let key = item.properties.name
  let val = item.properties.cp
  geoCoordMap[key] = val
})

mounted () {
    this.initMap()
  }


async initMap () {
      // 渲染地图
      let myChart = this.$echarts.init(document.getElementById('my_echarts'), 'light')
      myChart.setOption(this.options)
      // 获取数据
      let { code, map_data: mapData } = await getMapData()
      if (code === 0) {
        if (mapData.length > 0) {
          // 计算展示的大小比例
          mapData.sort((obj1, obj2) => { return obj2.quantity - obj1.quantity })
          let max = mapData[0].quantity
          let scale = max / 30
          mapData.map(item => {
            item.size = item.quantity / scale
          })

          // 将数据转为坐标数据
          let data = this.convertData(mapData)
          this.options.series.data = data
        }
        // 重新渲染echarts
        myChart.setOption(this.options)
      } else {
        let msg = this.$ERROR_MSG(code)
        this.$message.error(msg)
      }
    }


data中配置options
      options: {
        tooltip: {
          trigger: 'item',
          formatter: (item) => {
            return `${item.name}：${item.value[2]}`
          }
        },
        geo: {
          map: 'china',
          label: {
            emphasis: {
              show: false
            }
          },
          layoutCenter: ['50%', '50%'],
          layoutSize: 700,
          roam: false,
          itemStyle: {
            normal: {
              borderWidth: 1,
              areaColor: '#f1f8ff',
              borderColor: '#B7D8FF'
            },
            emphasis: {
              areaColor: '#f1f8ff',
              borderColor: '#B7D8FF'
            }
          }
        },
        series: {
          type: 'effectScatter',
          coordinateSystem: 'geo',
          symbol: 'image://data:image/png;base64,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',
          symbolSize: function (val) {
            return val[3]
          },
          showEffectOn: 'emphasis', // 'render'
          rippleEffect: {
            brushType: 'strock',
            // period: 10,
            scale: 1
          },
          hoverAnimation: true,
          itemStyle: {
            normal: {
              color: '#C4972E',
              // shadowBlur: 10,
              shadowColor: '#C4972E'
            }
          },
          zlevel: 1
        }
      }
```



#### Vue中获取dom

```js
<div id="main"></div>

this.$echarts.init(document.getElementById('main'))
```

因为vue是单页面应用，如果将以上的组件使用两次，一个页面内id是不允许相同的，就会出现第一个组件正常显示，第二个组件无法显示。

```js
<div ref="main"></div>

this.$echarts.init(this.$refs.main)
或
this.$echarts.init(this.$refs['main'])
```

通过以上方法获取dom，无论组件复用多少次，都不需要担心id唯一的问题。





