# 高德地图

## utils\getAMap.js

```javascript
export function getAMap() {
  return new Promise((resolve, reject) => {
    if (window.AMap) {
      resolve(window.AMap)
    } else {
      var script = document.createElement('script')
      script.type = 'text/javascript'
      script.src =
        'https://webapi.amap.com/maps?v=1.4.15&key=1b2d9a3af752f0d41ff596557d8ce55e&plugin=AMap.ToolBar.Geocoder&callback=initAMap'
      script.onerror = reject
      document.head.appendChild(script)
    }
    window.initAMap = () => {
      resolve(window.AMap)
    }
  })
}
```

## 使用

```html
<template>
  <div id="app">
    <div id="container"></div>
  </div>
</template>
<script>
import { getAMap } from '@/utils'
export default {
  mounted() {
    this.initAMap()
  },
  methods: {
    async initAMap() {
      let AMap = await getAMap()
      this.AMap = AMap
      let map = new AMap.Map('container', {
        zoom: 10,
        center: [116.39, 39.9]
      })
      // 同时引入工具条插件，比例尺插件和鹰眼插件
      AMap.plugin(
        [
          'AMap.ToolBar',
          'AMap.Scale',
          'AMap.OverView',
          'AMap.MapType',
          'AMap.Geolocation'
        ],
        function() {
          // 在图面添加工具条控件，工具条控件集成了缩放、平移、定位等功能按钮在内的组合控件
          map.addControl(new AMap.ToolBar())
          // 在图面添加比例尺控件，展示地图在当前层级和纬度下的比例尺
          map.addControl(new AMap.Scale())
          // 在图面添加鹰眼控件，在地图右下角显示地图的缩略图
          map.addControl(new AMap.OverView({ isOpen: true }))
          // 在图面添加类别切换控件，实现默认图层与卫星图、实施交通图层之间切换的控制
          map.addControl(new AMap.MapType())
          // 在图面添加定位控件，用来获取和展示用户主机所在的经纬度位置
          map.addControl(new AMap.Geolocation())
        }
      )
    },
    // 构建矢量圆形
    addCircle(lng, lat) {
      return new this.AMap.Circle({
        center: new this.AMap.LngLat(lng, lat), // 圆心位置
        radius: 500, //半径，米
        strokeColor: '#F33', //线颜色
        strokeOpacity: 1, //线透明度
        strokeWeight: 3, //线粗细度
        fillColor: '#ee2200', //填充颜色
        fillOpacity: 0.35 //填充透明度
      })
    },
    addMarker(myIcon, position) {
      let icon =
        myIcon || 'https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png'
      return new this.AMap.Marker({
        //官方icon自定义icon二选一
        // 官方icon
        icon,
        //自定义icon
        // icon: this.addIcon(),
        position
        // offset: new this.AMap.Pixel(-13, -30)
      })
    },
    //自定义icon
    myIcon() {
      return new this.AMap.Icon({
        // 图标尺寸
        size: new this.AMap.Size(40, 40),
        // 图标的取图地址
        // image: require('@/assets/auto.png'),
        // 图标所用图片大小
        imageSize: new this.AMap.Size(40, 40)
        // 图标取图偏移量
        // imageOffset: new this.AMap.Pixel(-13, -60)
      })
    }
  }
}
</script>

<style>
#container {
  height: 500px;
  width: 500px;
}
</style>
```
