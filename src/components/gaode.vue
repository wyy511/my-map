<template lang="pug">
section.gaode-wrap
  map-language(
    @change-lang="changeLang"
  )
  map-keysearch(
    @search="keysearch"
  )
  .gaode-container#container
</template>
<script>
import * as config from '../config'
import MapLanguage from '@/components/language.vue'
import MapKeysearch from '@/components/keysearch.vue'
  export default {
    data () {
      return {
        AMapUI: null,
        AMap: null,
        gaodeMap: null,
        language: 'zh_cn',
        placeSearch: null, // 关键字搜索
        searchLists: [],
        searchWord: ''
      }
      curPosition: {}
    },
    components: {
      'map-language': MapLanguage,
      'map-keysearch': MapKeysearch
    },
    async created() {
      // 已载入高德地图API，则直接初始化地图
      if (window.AMap && window.AMapUI) {
        // 获取当前位置的经纬度
        this.initMap()
        // this.getGeolocation()
      } 
    },
    methods: {
      initMap () {
        // 加载地图
        this.loadMap()
      },
      // 加载地图
      loadMap () {
        // 加载PositionPicker，loadUI的路径参数为模块名中 'ui/' 之后的部分
          let AMapUI = this.AMapUI = window.AMapUI
          let AMap = this.AMap = window.AMap
          let MapCityName = '北京'
          AMapUI.loadUI(['misc/PositionPicker'], PositionPicker => {
            let mapConfig = {
              zoom: 16,
              resizeEnable: true
            }
            if (this.lat && this.lng) {
              mapConfig.center = [this.lng, this.lat]
            }
            this.gaodeMap = new AMap.Map('container', mapConfig)
            // 地图加载完后进行语言切换
            this.changeLang(this.language)
            // 进行定位
            this.getGeolocation()
            //定义地图上的鼠标样式
            this.setMapCursor('crosshair')
            //地图中添加地图操作ToolBar插件
            AMap.plugin(['AMap.ToolBar'], () => {
                //设置地位标记为自定义标记
                var toolBar = new AMap.ToolBar();
                this.gaodeMap.addControl(toolBar);
            });
          })
      },
      // 进行定位
      getGeolocation () {
        this.gaodeMap.plugin('AMap.Geolocation', () => {
            let geolocation = new AMap.Geolocation({
                enableHighAccuracy: true,//是否使用高精度定位，默认:true
                timeout: 10000,          //超过10秒后停止定位，默认：无穷大
                maximumAge: 0,           //定位结果缓存0毫秒，默认：0
                convert: true,           //自动偏移坐标，偏移后的坐标为高德坐标，默认：true
                showButton: true,        //显示定位按钮，默认：true
                buttonPosition: 'LB',    //定位按钮停靠位置，默认：'LB'，左下角
                buttonOffset: new AMap.Pixel(10, 20),//定位按钮与设置的停靠位置的偏移量，默认：Pixel(10, 20)
                showMarker: true,        //定位成功后在定位到的位置显示点标记，默认：true
                showCircle: true,        //定位成功后用圆圈表示定位精度范围，默认：true
                panToLocation: true,     //定位成功后将定位到的位置作为地图中心点，默认：true
                zoomToAccuracy:true      //定位成功后调整地图视野范围使定位位置及精度范围视野内可见，默认：false
            });
            this.gaodeMap.addControl(geolocation);
            geolocation.getCurrentPosition();
            // console.log(geolocation)
            AMap.event.addListener(geolocation, 'complete', this.onComplete);//返回定位信息
            AMap.event.addListener(geolocation, 'error', this.onError);      //返回定位出错信息
        });
      },
      // 进行语言切换
      changeLang (id) {
        this.gaodeMap.setLang(id);
      },
      //定义地图上的鼠标样式
      setMapCursor (type) {
        this.gaodeMap.setDefaultCursor(type)
      },  
      // 按关键字搜索
      keysearch (data) {
        var MSearch;  
        this.gaodeMap.plugin(["AMap.PlaceSearch"], () => {          
            MSearch = new AMap.PlaceSearch({ //构造地点查询类  
                pageSize:10,  
                pageIndex:1,  
                city:"021" //城市  
            });   
            AMap.event.addListener(MSearch, "complete", this.getSearchPlace);//返回地点查询结果  
            MSearch.search(data.val); //关键字查询  
        });  
      },
      
      getSearchPlace(data) {
        this.searchLists = data.poiList.pois
        console.log(this.searchLists)
      },
      //解析定位结果
      onComplete (data) {
        console.log(data)
        var str=['定位成功'];
        str.push('经度：' + data.position.getLng());
        str.push('纬度：' + data.position.getLat());
        if(data.accuracy){
              str.push('精度：' + data.accuracy + ' 米');
        }//如为IP精确定位结果则没有精度信息
        str.push('是否经过偏移：' + (data.isConverted ? '是' : '否'));
        console.log(str.join('<br>'))
        // document.getElementById('tip').innerHTML = str.join('<br>');
      },
      //解析定位错误信息
      onError (data) {
        console.log('定位失败')
        // document.getElementById('tip').innerHTML = '定位失败';
      }
    }
  }
</script>
<style lang="scss">
.gaode-wrap {
  position: relative;
  width: 100%;
  .gaode-container {
    width: 100%;
    height: 500px;
  }
}
</style>