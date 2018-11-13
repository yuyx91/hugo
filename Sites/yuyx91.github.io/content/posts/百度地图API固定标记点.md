---
title: 百度地图API固定标记点
date: 2017-02-22 10:25:25
tags: "api"
---
### 1.首先注册百度地图API，创建应用，获取ak码。
<a href="http://lbsyun.baidu.com/" title="百度地图API" target="_blank">百度地图API</a>
<!--more-->
### 2.HTML页面布局
```html
    <div id="mymap" style="width:100%;height:500px;overflow:hidden"></div>
```
### 3.引入带有ak密钥的js
```js
    <script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak=GjRyiHdbmru1jctFIodetnZQ"></script>
```
### 4.创建js配置地图
```js
    <script>
            // 百度地图API功能
            //创建map地图实例
            var map = new BMap.Map('mymap');
            //设置中心点，固定位置：百度大厦
            var poi = new BMap.Point(116.307852,40.057031);
            map.centerAndZoom(poi, 16);
            //启用滚轮放大缩小，默认禁用
            map.enableScrollWheelZoom();
            //启用地图惯性拖拽，默认禁用
            map.enableContinuousZoom();
            //创建信息展示窗口
            var content = '<div style="margin:0;line-height:20px;padding:2px;">' +
                            '<img src="../img/baidu.jpg" alt="" style="float:right;zoom:1;overflow:hidden;width:100px;height:100px;margin-left:3px;"/>' +
                            '地址：北京市海淀区上地十街10号<br/>电话：(010)59928888<br/>简介：百度大厦位于北京市海淀区西二旗地铁站附近，为百度公司综合研发及办公总部。' +
                          '</div>';

            //创建检索信息窗口对象
            var searchInfoWindow = null;
        	searchInfoWindow = new BMapLib.SearchInfoWindow(map, content, {
        			title  : "百度大厦",      //标题
        			width  : 290,             //宽度
        			height : 105,              //高度
        			panel  : "panel",         //检索结果面板
        			enableAutoPan : true,     //自动平移
        			searchTypes   :[
        				BMAPLIB_TAB_SEARCH,   //周边检索
        				BMAPLIB_TAB_TO_HERE,  //到这里去
        				BMAPLIB_TAB_FROM_HERE //从这里出发
        			]
        		});
        	//创建标注
            var marker = new BMap.Marker(poi); //创建marker对象
            marker.enableDragging(); //marker可拖拽
            marker.addEventListener("click", function(e){
        	    searchInfoWindow.open(marker);
            })
            map.addOverlay(marker); //在地图中添加marker
          	marker.setAnimation(BMAP_ANIMATION_BOUNCE);//设置标注跳动动画
    </script>
```
### 5.示例
<div id="mymap" style="width:100%;height:500px;overflow:hidden">
<script src="http://api.map.baidu.com/api?v=2.0&ak=GjRyiHdbmru1jctFIodetnZQ"></script>
<script>
            var map = new BMap.Map("mymap");
            			var point = new BMap.Point(113.936253,22.542715);
            			map.centerAndZoom(point, 17);
            		 	map.enableScrollWheelZoom();
            			map.enableContinuousZoom();
            			var marker = new BMap.Marker(point);
            			map.addOverlay(marker);
            			marker.setAnimation(BMAP_ANIMATION_BOUNCE);
            			var opts = {
            			  width : 200,     // 信息窗口宽度
            			  height: 50,     // 信息窗口高度
            			  title : "深圳市荔香公园" , // 信息窗口标题
            			}
            			var infoWindow = new BMap.InfoWindow("地址：深圳市南山区南光路与南头街交界处", opts);  // 创建信息窗口对象
            			marker.addEventListener("click",function(){
            				map.openInfoWindow(infoWindow,point);
            			})
    </script>
