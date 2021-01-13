# GIS学习小组练习#3
2021-1-13

## 目标：
掌握基础的Spatial Analysis的概念和操作。主要包括两部分内容：

### Spatial Query（Selection by Location）：
确认spatial objects之间的空间关系，选择符合所需空间关系的spatial objects。常见的空间关系包括touch, within, contains, intersects等。

例如，已知A、B、C三座城市的地理信息（三个polygon），挑选出B、C中与A相邻的城市。在这个例子中，A城市是Target，B、C城市是待选择的对象，我们要选择与目标符合一定空间关系的对象（此例为touch关系）。

再例如，已知A城市和#1-#10十所中学的地理信息（一个polygon和十个point），挑选出A城市覆盖的中学（此例为contains关系）。

### Spatial Join：
基于spatial objects的空间关系合并数据。常见的用于合并数据的空间关系包括within, contain, intersects, closest distance等。

例如，已知（1）城市数据——A、B、C三座城市的地理信息和中学生总数，以及（2）中学数据——#1-#30共三十所中学的地理信息和各中学的学生数，但不知道各中学所处的城市；我们想知道各中学学生数占所处城市中学生总数的比例。我们可以利用Spatial Join将中学数据与城市数据合并起来。在这个例子中，中学数据是Target，我们希望能够在这个数据中添加所处城市中学生总数这个信息，而中学生总数这个信息保存在城市数据这个待Join的对象中，那么利用Spatial Join就可以基于空间关系将两个数据合并起来（此例为within关系）。

需要注意的是，无论在ArcGIS还是Python中操作，Spatial Join输出的数据中geometry这一列都是Target的geometry信息，在这个例子中，Spatial Join的结果只包含中学的geometry信息（30个point），而不会有城市的geometry信息（3个polygon）。当然了，将城市数据作为Target，中学数据作为待Join的对象也可以实施Spatial Join，只不过得到的结果不同。所以要根据自己的目标，确定应将哪个数据作为Target，哪个数据作为待Join的对象。牢记Spatial Join输出空间数据保留的是Target的geometry，这样有利于作出正确的判断。


## 材料

###	Spatial Analysis的概念和类别：
ArcGIS教材第8章对Spatial Query和Spatial Analysis都进行了详细介绍。由于Python的材料中没有类似的系统介绍的内容，所以建议无论是使用ArcGIS还是Python都先参考ArcGIS教材第8章了解概念。

###	Spatial Analysis的操作
####	ArcGIS操作：
请按照教材步骤完成第8章的操作，并回答其中的问题。所需数据已上传GitHub。两周后提供答案和成图样例。
相较来说，ArcGIS提供更为丰富的可以直接使用的Spatial Query方法，尤其是基于距离的Spatial Query，例如选择所有距离A城市小于100公里的高速公路。在Python中进行类似的操作需要更多的步骤。
####	Python操作：
1. 简单的Satial Query
依靠Shapely的Binary Predict实现。请跟着Tutorial的[Point in Polygon & Intersect](https://automating-gis-processes.github.io/site/notebooks/L3/point-in-polygon.html)完成操作。此外可以参考[Shapely User Manual](https://shapely.readthedocs.io/en/stable/manual.html#spatial-analysis-methods)中Binary Predict的部分。
2.	Spatial Join中的within, contains, intersects
依靠GeoPandas的sjoin()实现。请跟着Tutorial的[Spatial Join](https://automating-gis-processes.github.io/site/notebooks/L3/spatial-join.html)部分完成操作。此外可以参考GeoPandas手册的[Merging Data](https://geopandas.org/mergingdata.html)部分。
3.	基于nearest distance的Spatial Join
依靠Shapely的nearest_points()实现。请跟着Tutorial的[Nearest Neighborhood Analysis](https://automating-gis-processes.github.io/site/notebooks/L3/nearest-neighbour.html)部分完成操作。此外可以参考[Shapely User Manual](https://shapely.readthedocs.io/en/stable/manual.html#spatial-analysis-methods)中nearest_points()的部分。值得注意的是，nearest_points()的输入不一定是point/multipoint，也可以是line/multiline, polygon/multipolygon，无论输入哪种类型的geometry，nearest_points()总是给出最相近的那两个点的位置。例如，如果输入pointA和multilineB，那么nearest_points()就会返回两个点：pointA，multiline中与pointA最近的那个点。

### 注意坐标系统
所有Spatial Analysis中涉及到距离的操作（例如将距离各中学最近的公交车站的站名、车次信息Join到中学数据中），都要使用恰当的Projected Coordinate System（适合所分析的地理区域），保证距离衡量的准确度。请留意ArcGIS教材第8章以及Python Tutorial中的数据的坐标系统。
