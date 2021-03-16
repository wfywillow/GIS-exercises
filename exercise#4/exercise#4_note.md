# GIS学习小组练习#4
2021-3-16

## 目标：
掌握常用的Geoprocessing的概念和操作。Geoprocessing和Exercise#3中的Spatial Analysis同是GIS分析的基石，但这两者有重要的区别。

Spatial Analysis中的Spatial Query的目的是根据spatial objects之间的空间关系选择数据；Spatial Join的目的是根据空间关系合并数据，注意这一操作并不合并geometry本身，只合并geometry的Attributes，即只是将Join的对象所附带的numeric或text信息赋予到Target上，所以并不会产生新的geometry——输出结果的geometry就是原始的Target的geometry。例如，数据A表示城市，它包含的geometry是代表城市的polygon，附带的信息是这些城市的名称；数据B表示洪泛区（flood plain），它包含的geometry是代表洪泛区的polygon，附带的信息是这些洪泛区的编号。以数据A为Target，数据B为Join的对象，用intersects关系进行Spatial Join，输出的结果所包含的geometry依然是代表城市的polygon，但是每一条polygon都增加了一项信息——与之相交的洪泛区的编号。

与Spatial Analysis不同的是，Geoprocessing输出的结果会产生新的geometry。例如，以数据A和B为输入进行intersect类的Overlay，输出结果的geometry既和A不同也和B不同，而是A和B相交所形成polygon——城市和洪泛区的相交部分，输出结果的Attributes会同时包含城市名称和洪泛区标号。因此要根据操作的目的，确定是进行Spatial Analysis还是Geoprocessing。

常用的Geoprocessing包括：
* Overlay
* Buffer
* Merge
* Clip
* Dissolve

## 材料：
ArcGIS教材第9章和[AutoGIS Tutorial Lesson 4](https://automating-gis-processes.github.io/site/lessons/L4/overview.html)。ArcGIS教材介绍更为详细，所以建议无论是使用ArcGIS还是Python都先参考ArcGIS教材第9章了解概念。
### ArcGIS操作
按照教材步骤完成第9章的操作，并回答其中的问题。所需数据已上传GitHub。两周后提供答案和成图样例。
### Python操作
除了Tutorial Lesson 4的内容外，需要参考以下材料：
#### Overlay
GeoPandas手册[Set Operations with overlay](https://geopandas.org/docs/user_guide/set_operations.html)
#### Buffer 
GeoPandas手册[Geometric Manipulations-Constructive Methods](https://geopandas.org/docs/user_guide/geometric_manipulations.html)
#### Merge
Geopandas手册[Merging Data-Appending](https://geopandas.org/docs/user_guide/mergingdata.html)
#### Clip
GeoPandas手册[Clip Vector Data with GeoPandas](https://geopandas.org/gallery/plot_clip.html)
#### Dissolve
GeoPandas手册[Aggregation with dissolve](https://geopandas.org/docs/user_guide/aggregation_with_dissolve.html)

## 注意：
ArcGIS教材第9章Step 9.1 Getting Started部分介绍了environment settings；虽然只是简略的介绍，但建议不要忽略。environment settings中的设置会对所有的Geoprocessing结果产生影响，所以进行正确的设置可以避免错误而且让操作变得更高效，比如可以统一设置output的路径，output所用的坐标系统。
