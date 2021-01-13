# GIS学习小组练习#2
2020/12/30

纽约市共享单车站点作图

##	数据来源
【数据分享】纽约花旗共享单车轨迹数据（2013-2020）
下载2020年11月数据

##	目标
作图表示纽约市共享单车2020年11月骑行路程终点站点的地理位置。

##	技能
*	根据地点的经纬度生成GIS数据。
*	为所制作图层搭配合适的底图。
*	根据任务（步骤）的目的判断应使用的坐标系统。

##	需要的GIS包
geopandas, shapely.geometry, contextily

## 辅助信息
* 关于根据经纬度生成GIS数据，使用Python的操作方法在上一次练习中已提到，若在ArcMap中操作，要通过Add XY Data来实现。不论使用那种工具，注意X指的是经度，Y指的是维度。
*	关于通过网络资源直接加载底图，建议参考以下材料：
    -	使用ArcMap可以通过Add Basemap或者Add from ArcGIS Online添加底图，请参考**ArcGIS教材第5章**。
    -	如果使用Python，可以调用contextily (context geo tiles in Python)包。[Contextily包的网站](https://contextily.readthedocs.io/en/latest/intro_guide.html)讲解很清晰，可以结合[Tutorial L5-static maps](https://automating-gis-processes.github.io/site/notebooks/L5/static_maps.html)学习（Tutorial中的代码虽然依然可以运行，但是对应的是较旧版本的contextily,尽量还是以contextily网站讲解为准）。有一个窍门是：通过参数zoom设定底图的清晰度，可以提高获取底图的速度（当然前提是没有把清晰度设得特别高）。
*	此次练习依然涉及到坐标系统的确认和选择，如果对坐标系统的相关问题（例如geographic coordinate system和projected coordinated system的关系和区别）依然有疑惑，非常建议学习Coursera课程[Introduction to GIS Mapping](https://www.coursera.org/learn/introduction-gis-mapping/home/)的Week4和Week5。确认和选择坐标系统是GIS操作的基本内容，如果能够形成习惯、轻松做到，可以避免以后遇到复杂的任务时花费时间或出现错误。   
    - 原始数据中地理信息以经纬度的形式给出，因此依此生成GIS数据时应选用GCS坐标系。我们接触到的公开的GIS数据所最常用的GCS坐标系是WGS84（也有一定可能是NAD83），所以虽然数据源没有说明，我们可以很有把握地认为此数据中的经纬度对应的是WGS84坐标系。
    - 使用Python作图时我们的站点图层和底图要保持坐标系一致。使用contextily添加底图时，底图默认采用的是WGS 84 / Pseudo-Mercator（代码EPSG:3857），但是可以通过参数“crs”来设定采用其他的坐标系。我们的站点图层的坐标也是可以转换的。因此有很多种操作的选择使站点图层和底图的坐标系相同（可以都是WGS 84，也可以都是WGS 84 / Pseudo-Mercator，或者都是某一其他坐标系）。当然，一般来说使用WGS84坐标系制作的图的形状不太符合我们平时看到的地图（因为它是未经过投影的），由于此次练习的目标是制作地图显示位置而不是计算，所以可以选择一个一般地图常用的、看着舒服的坐标系。
 - ArcMap中Data Frame的坐标系由第一个添加进Data Frame的数据的坐标系所决定。比如首先向Data Frame添加了我们坐标系为WGS84的共享单车终点站点数据，那么这个Data Frame的坐标系就会是WGS84。之后在同一Data Frame中添加底图的话，ArcMap会自动将底图显示成WGS84坐标系下的样子。当然，可以通过Data Frame的Property来更改它的坐标系，这样它的坐标系就不受哪个数据是第一个添加进来的影响了。值得注意的是，上面所说这些坐标系的变化都只涉及数据在ArcMap窗口中看起来的样子，不会更改数据本身的坐标系。
* 布局一幅informative的地图的具体步骤可以参考**ArcGIS教材第3章**。一幅最标准的地图应该包括哪些要素，可以参考[ArcGIS官网一个短课程](https://learn.arcgis.com/en/projects/get-started-with-arcmap/)结尾的成图。虽然很多使用场景下，一幅没有这么标准的地图也能被接受，但是尽量让成品地图除了所制作的图层和底图以外包括这些内容：title，north arrow，scale bar，legend，note（可以包括关于数据来源、制图者、制图时间、坐标系统、对图的内容的解释等等的信息。）（因为用ArcMap很方便添加这些要素，我目前习惯于用ArcMap来作最后的成图layout。）

##	推荐ArcGIS短课程
即上面提到的[短课程](https://learn.arcgis.com/en/projects/get-started-with-arcmap/)。这一个小的project涵盖了很多常用的GIS操作（包括添加底图），其中spatial join和geoprocessing这两个重要的内容我们会在之后的练习中覆盖，现在可以通过这个短课程提前接触。另外，在“Create an inset map”这部分，作者展示了如何通过customize坐标系来提高地图的可读性。
