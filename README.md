### Openlayer4加载ArcGIS离线瓦片地图

> 本来以前是用openlayer2,在太乐地图下载的地图,会有模版.之前直接在此基础上更改的代码,但是随着项目的发展功能的增多,openlayer2越来越不适应现在的项目,所以决定换成openlayer4,今天给大家说的是Openlayer4加载ArcGIS离线瓦片地图.
#### 步骤1:下载地图(我用的是太乐地图下载器)
![下载地图](http://img.blog.csdn.net/20180112114545773?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjcxMTg4OTU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
##### 由于大小的原因,我们选择前6级下载
![选择层级](http://img.blog.csdn.net/20180112114654171?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjcxMTg4OTU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 步骤2:导出ArcGIS地图
![导出ArcGIS地图](http://img.blog.csdn.net/20180112115014477?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjcxMTg4OTU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 步骤3:复制出我们想要的地图瓦片文件
![复制出我们想要的地图瓦片文件](http://img.blog.csdn.net/20180112115319148?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjcxMTg4OTU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这个样子的
![样子](http://img.blog.csdn.net/20180112115744972?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjcxMTg4OTU=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
#### 步骤4:代码这样写,里面有注释可以修改

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>ol加载ArcGIS本地切片</title>
    <link rel="stylesheet" href="js/ol.css">
    <script src="js/ol.js"></script>
</head>
<body>
<div id="map"></div>
<script type="text/javascript">
    // 设置地图中心，此处进行坐标转换， 把EPSG:4326(经纬度坐标)，转换为EPSG:3857(WGS84网络墨卡托(辅助球))坐标，因为ol默认使用的是EPSG:3857坐标
    //[106.677, 36.7388]是下载地图的中心经纬度
    var centerPos = ol.proj.transform([106.677, 36.7388], 'EPSG:4326', 'EPSG:3857');

    //创建地图
    var map = new ol.Map({
        view: new ol.View({
            center: centerPos,//地图中心位置
            zoom: 0//地图初始层级
        }),
        //添加地图图层
        //这里注销在下面添加新的离线地图图层
        /*layers: [
           new ol.layer.Tile({
                source:new  ol.source.OSM()
           })
       ],*/
        //将地图添加到的map容器中
        target: 'map'
    });

    //给8位字符串文件名补0
    function zeroFill(num, len, radix) {
        var str = num.toString(radix || 10);
        while (str.length < len) {
            str = "0" + str;
        }
        return str;
    }

    // ol.source.XYZ添加瓦片地图的层
    var tileLayer = new ol.layer.Tile({
        source: new ol.source.XYZ({
            tileUrlFunction: function (coordinate) {
                //alert(coordinate[0] + " X= " + coordinate[1] + " Y= " + coordinate[2]);
                var x = 'C' + zeroFill(coordinate[1], 8, 16);
                var y = 'R' + zeroFill(-coordinate[2] - 1, 8, 16);
                var z = 'L' + zeroFill(coordinate[0], 2, 10);
                return '_alllayers/' + z + '/' + y + '/' + x + '.jpg';//这里可以修改地图路径
            },
            projection: 'EPSG:3857'
        })
    });
    map.addLayer(tileLayer);//添加到map里面
</script>
</body>
</html>
```
### 源码下载地址:https://github.com/1294083463/openlayersLoadArcGIS




## 作者
#### 作者: 孟庆岳
#### 官网: 百度搜索([shmily科技](http://weareshmily.top/ "shmily科技"))
#### CSDN博客:http://blog.csdn.net/qq_27118895
#### github:https://github.com/1294083463/openlayersLoadArcGIS