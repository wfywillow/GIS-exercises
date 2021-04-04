# GIS小组练习#5
2021-04-04

## 目标
作图显示纽约市共享单车骑行终点周边的某一类POI（例如，餐馆、学校）的个数

## 数据来源

### 纽约市共享单车骑行数据
从[【数据分享】纽约花旗共享单车轨迹数据（2013-2020）](https://mp.weixin.qq.com/s?__biz=MzA3NTk0MTU3MA==&mid=2247504214&idx=1&sn=45cabb2f8b7352b9b34800dabb3d9735&chksm=9f6a42fba81dcbed8af62fe2e0835c7a3c0b4eb861d2252dd58e510c659c98af7c0ec73467cb&scene=126&sessionid=1607993471&key=2f88c2a11d638eea38954844950eb3d4a0d2e474fe5aa2b1b058bb12d0ad38dce02a63d1d5c001db9df6c71199a5db630d077c74628b335e19e77b0b2db9d470e9fb8723377741fbbf59609e5e06611285a5dab60c19d6061fece81d66aa62c8f1688d66512bddc694bec941bbbc170e9b2d2d7af334cefb2b8590598959fe1c&ascene=1&uin=Mjc3MTE5MzQ2Mg%3D%3D&devicetype=Windows+10+x64&version=6300002f&lang=zh_CN&exportkey=AUZZSfMZfW8IaSqkkob7XEI%3D&pass_ticket=kODwkd39ekHcduyOZOoHVP1sKpNPlAe2MDfWDkUAfb0qUlQc9G2ydfqVHCpu0%2Fs4&wx_header=0)下载2020年11月数据。

### 纽约市地区边界数据
从[纽约市城市规划部门网站](https://www1.nyc.gov/site/planning/data-maps/open-data/districts-download-metadata.page)下载State Assembly Districts (Clipped to Shoreline)数据。

## 步骤
1. 基于单车骑行数据中骑行终点的经纬度生成骑行终点的GIS数据。
2. 确定自己所感兴趣的一类POI（例如，餐馆、学校），通过OSMnx包从Open Street Map获得这类POI的GIS数据。
3. 确定在各骑行终点周边多远距离内（即radius）寻找POI。
4. 按照第3步确定的radius为各骑行终点生成buffer（buffer是GeoProcessing的一种，参见exercise#4）。
5. 通过Spatial Join（参见exercise#3）找出各个骑行终点周边的POI。
6. 计算各个骑行终点周边POI的个数。
7. 使用骑行终点数据和纽约市地区边界数据制作地图。
> 要求：（1）代表骑行终点的点的颜色越深表示其周边的该类POI越多；（2）用legend或colorbar说明点的颜色所代表的数值；（3）纽约市地区边界作为底图，方便看图的人对骑行终点的位置有大体的感知。

## 材料
### OMSnx包
1. [Tutorial Lesson 6 - Retrieving OpenStreetMap data](https://automating-gis-processes.github.io/site/notebooks/L6/retrieve_osm_data.html)
2. [OMSnx手册](https://osmnx.readthedocs.io/en/stable/#)，尤其是osmnx.geometries module部分
> 先参考Tutorial；由于Tutorial中的很多用法已经不再被支持了，自己操作之前需要再仔细参考OMSnx手册。
> 虽然Tutorial主要是通过城市名称从OMSnx获取数据，但其实还有其他操作方法。比如，可以划定东、南、西、北的界限，或者划定一个polygon作为界限，尽量尝试下其他方法。（注意，所划定的边界必须采用WGS84经纬度格式。）

### 制作地图
1. [GeoPandas Mapping and Plotting Tools](https://geopandas.org/docs/user_guide/mapping.html)
2. [GeoPandas手册geopandas.GeoDataFrame.plot](https://geopandas.org/docs/reference/api/geopandas.GeoDataFrame.plot.html)
> 注意geopandas.GeoDataFrame.plot如何添加colorbar或legend。

### Nearest Neighbour Analysis
是否需要用到Nearest Neighbour Analysis取决于确定radius的方法。当然可以凭自己的主观喜好确定radius，也可以选择一个更客观的标准。

有一种可行的方法是：针对每一个骑行终点，找到与它最近的其他骑行终点，计算最近距离，考察所有最近距离的分布，选择均值或中位数的一半作为radius。

如果采用这一方的话，就需要用到Nearest Neighbour Analysis。可参考的材料包括：
1. [Tutorial Lesson 3 - Nearest Neighbour Analysis](https://automating-gis-processes.github.io/site/notebooks/L3/nearest-neighbour.html)
2. [Shapely手册Nearest Points](https://shapely.readthedocs.io/en/stable/manual.html)

## 注意事项
### 坐标系统
由于这项任务的核心是寻找骑行终点周边一定长度距离内的POI，也就是涉及到以长度衡量的距离问题，所以应使用一个适合纽约市的、可以保持距离的PCS（projected coordinated system），比如WGS84/UTM zone 18N

所有的数据——单车骑行终点、POI、地区边界——都需要转化至这一坐标系统。查询数据当前的坐标系统和转换坐标系统的方法参见[Tutorial Lesson 2 - Map projections](https://automating-gis-processes.github.io/site/notebooks/L2/projections.html)

### 地图的extent
单车骑行终点所覆盖的地理范围远小于整个纽约市的地理范围；如果不加限制的话，制作出的地图会显示出整个纽约市，而我们所关心的单车骑行终点只占据地图的很小一部分，难以清晰地显示。

所以制作地图时要限制x方向和y方向地最大值和最小值（也就是东、西、南、北的边界）。

当然，另一种更麻烦的方法是将纽约市地区边界数据clip至和骑行终点数据的范围（覆盖所有点的最小矩形或者convex hull）相同，然后再作图。
