---
uuid: 57058550-2bb2-0556-34a3-b70883f0ee01
title: Nuxt.js使用百度地图vue-baidu-map
date: 2020-06-19 13:48:48
tags: Nuxt
category: Nuxt
---

##  vue-baidu-map文档：https://dafrok.github.io/vue-baidu-map/#/zh/start/usage
<!-- more -->
安装vue-baidu-map
```
npm i vue-baidu-map -D
```
在plugins新建map.js:
```
import BaiduMap from 'vue-baidu-map'
import Vue from 'vue'
Vue.use(BaiduMap, {
  ak: '申请的百度地图密匙'
})
```
在nuxt.config.js中引入：
```
  plugins: [
    { src: "~plugins/map.js", ssr: false },
  ],
```
在页面中使用：
```html
<template>
  <div>
    <baidu-map class="bdwindow" :dragging="dragging" :center="center" :zoom="zoom" style="height:500px" :scroll-wheel-zoom='scroll'>
          <bm-info-window :position="center" :title="title" :show="show">
              <p class="bdwtext" v-html="contents"></p>
          </bm-info-window>
    </baidu-map>
  </div>
</template>
<script>
export default {
  data () {
        return {
            jump_path:"",
            center: {lng: 120.4373010751, lat: 23.1095638170},
            zoom: 15,  //缩放级别
            title:"标题",
            contents: '地址：具体地址信息',  //标签内容
            show: true,  //显示标签
            scroll:true,  //地图缩放
            dragging:true,  //地图拖拽
        }
    }
}
</script>
```

##  自动获取地理位置 - 此地图修改性比较大， - 后续需要修改
问题点：keyword使用props传一个值过来，或者使用ref，来解决修改的问题

父组件使用： <vue-map-baidu-key @mapMessage="mapMessage"></vue-map-baidu-key>
```html
<template>
  <div>
    <!--地图模块-->
    <label class="row">
      <el-input class="col-6" v-model="keyword"></el-input>
      <el-button @click="centerDialogVisibleClick" plain>查找位置</el-button>
    </label>
    <el-dialog
      title="点击查看学校地址"
      :visible.sync="centerDialogVisible"
      width="1000px"
      top="2vh"
      center>
      <!-- getPoint方法，给地图加点击事件，点击地图获取所需要的信息，-->
      <!--scroll-wheel-zoom属性是否可以用鼠标滚轮控制地图缩放-->
      <baidu-map :scroll-wheel-zoom="true" :zoom="zoom" 
@click="getPoint" center="滨州市" style="width: 100%;height: 400px"
                 class="map" id="mapID">
        <!--地图类型，两种：一种是路线一种是绿的那种 :showAddressBar="true"是否显示默认的弹窗-->
        <bm-map-type :map-types="['BMAP_NORMAL_MAP', 'BMAP_HYBRID_MAP']"
                     anchor="BMAP_ANCHOR_TOP_LEFT"></bm-map-type>
        <!--地图搜索功能，绑定上面的input，-->
        <!--display: none样式很关键，因为下面默认会有地址提示信息很长，很烦，这样搜索会很舒服，-->
        <!--zoom是搜索结果的视图比例，个人觉得12.8很舒服显示-->
        <bm-local-search :auto-viewport="true"
                         :keyword="keyword"
                         :location="location" style="display: none"
                         zoom="12.8"></bm-local-search>
        <bm-navigation anchor="BMAP_ANCHOR_TOP_RIGHT"></bm-navigation>
        <!--      信息窗口，show属性是控制显示隐藏，infoWindowClose和infoWindowOpen是控制信息窗口关闭隐藏的方法-->
        <bm-marker :position="postionMap">
          <bm-info-window :show="show" @close="infoWindowClose" @open="infoWindowOpen" style="font-size: 14px">
            <p>学校名称：{{ add.jgName }}</p>
            <p>当前地址：{{ add.site }}</p>
          </bm-info-window>
        </bm-marker>
      </baidu-map>
      <!--      //显示数据-->
      <div slot="footer" class="dialog-footer row">
        <ul class="list-group-flush col-10 row text-left">
          <li class="map-baidu-xinxi col-6 pr-1">
            <input type="text" class="form-control col-10 border-0 "
                   placeholder="输入学校位置"
                   aria-label="Example text with button addon"
                   aria-describedby="button-addon1"
                   v-model="keyword"></li>
          <li class="map-baidu-xinxi col-3 pr-1">经度：{{add.postionMap.lng}}</li>
          <li class="map-baidu-xinxi col-3 pr-1">维度：{{add.postionMap.lat}}</li>
          <li class="map-baidu-xinxi col-3 pr-1"><b>省：</b>{{add.province}}</li>
          <li class="map-baidu-xinxi col-3 pr-1"><b>市：</b>{{add.city}}</li>
          <li class="map-baidu-xinxi col-3 pr-1"><b>区：</b>{{add.district}}</li>
          <li class="map-baidu-xinxi col-3 pr-1"><b>街：</b>{{add.street}}</li>
        </ul>
        <div class="col-2">
          <el-button class="col-10 ml-0 mb-1" type="primary" @click="centerDialogVisibleOK">确 定</el-button>
          <el-button class="col-10 ml-0" @click="centerDialogVisible = false">取 消</el-button>
        </div>
      </div>
    </el-dialog>

  </div>
</template>


<script>
  export default {
    data() {
      return {
        //显示地图红点上的信息
        show: false,
        //地图坐标
        postionMap: {
          lng: '',
          lat: ''
        },
        //搜索框关键词 - 重要
        keyword: '',
        //位置
        location: '',
        //放大比例
        zoom: 12.8,
        //位置详细信息
        address: '',
        // 返回父组件的东西 -
        add: {
          //站点名称 - 可不要 - 换成学校名
          siteName: '',
          //地址
          site: '',
          //经纬度
          postionMap: {
            lng: '',
            lat: ''
          },
          //备注说明
          desce: '',
          //类型
          type: '',
          //省
          province:'',
          //城市
          city:'',
          //区县
          district:'',
          //街道
          street:'',
        },
        centerDialogVisible: false
      }
    },
    methods: {
      //点击选取地图 打开弹窗
      centerDialogVisibleClick() {
        this.centerDialogVisible = true
      },
//点击确认之后关闭弹窗传出this.add
      centerDialogVisibleOK(){
        this.add.address = this.keyword
        this.centerDialogVisible = false
        this.$emit('mapMessage',this.add)
      },
      //点击地图获取一些信息，
      getPoint(e) {
        this.show = true
        this.postionMap.lng = e.point.lng     //通过  e.point.lng获取经度
        this.postionMap.lat = e.point.lat     //通过  e.point.lat获取纬度
        this.add.postionMap.lng = e.point.lng
        this.add.postionMap.lat = e.point.lat
        //创建地址解析器的实例
        let geocoder = new BMap.Geocoder()
        geocoder.getLocation(e.point, rs => {
          this.add.site = rs.address
          this.add.province = rs.addressComponents.province
          this.add.city = rs.addressComponents.city
          this.add.district = rs.addressComponents.district
          this.add.street = rs.addressComponents.street
          // 地址描述(string)=
          console.log(rs) //用什么可以在这里面找
          console.log(rs.address)    //这里打印可以看到里面的详细地址信息，可以根据需求选择想要的
          // console.log(rs.point) //获取坐标
          // console.log(rs.addressComponents)//结构化的地址描述(object)
          // console.log(rs.addressComponents.province) //省
          // console.log(rs.addressComponents.city) //城市
          // console.log(rs.addressComponents.district) //区县
          // console.log(rs.addressComponents.street) //街道
          // console.log(rs.addressComponents.streetNumber) //门牌号
          // console.log(rs.surroundingPois) //附近的POI点(array)
          // console.log(rs.business) //商圈字段，代表此点所属的商圈(string)
        })
      },
      infoWindowClose() {
        this.show = false
      },
      infoWindowOpen() {
        //这里有个问题纠结了很久，百度的信息窗口默认有个点击其他地方就消失的事件，我没有找到
        //并且信息窗口点击一次显示，一次消失
        //于是我加了一个100毫秒的定时器，保证每次点击地图都可以展示信息窗口
        setInterval(() => {
          this.show = true
        }, 100)
      }
    }
  }
</script>


<style scoped>
  .map-baidu-xinxi{
    display: block;
    border-bottom: 1px solid rgba(0,0,0,.125);
    line-height: 2.5;
  }
  input:focus{ box-shadow: none }
</style>
```

