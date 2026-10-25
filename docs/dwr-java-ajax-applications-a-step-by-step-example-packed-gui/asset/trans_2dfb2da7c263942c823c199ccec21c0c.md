
```
{
    html+='<li><a href="#" onclick="expandElement(this);return false;">'+element+'</a></li>';
}
else
{
    html+='<li style="background-color:#dddddd"><a href="#" onclick="showContent(this);return false;">'+element+'</a></li>';
}
});
});
var parentNode=element.parentNode;
var childNodes=parentNode.childNodes;
if(childNodes.length>1)
{
    for(var i=1;i<childNodes.length;i++)
    {
        parentNode.removeChild(childNodes[i]);
    }
    var treeMenuContent=dwr.util.byId('treeMenuContent');
    treeMenuContent.innerHTML="";
}
else
{
    var ul = document.createElement("ul");
    ul.innerHTML=html;
    ul.style.backgroundColor="#eeeeee";
    parentNode.appendChild(ul);
}

function showContent(element)
{
    var value=element.text.replace(' ','');
    NavigationTree.getContent(value,{
        callback:function(content)
        {
            var treeMenuContent=dwr.util.byId('treeMenuContent');
            treeMenuContent.innerHTML=content;
        }
    });
}
```

第一个函数是 getRootElements() Java 调用的回调函数。该函数的功能是获取根元素，并针对每个元素检查是否存在子元素，然后据此编写菜单元素的 HTML 代码。请注意，菜单是 li 元素，我们的 wireframemenu 样式规定了如何在菜单中处理这些 li 元素。我们看到的不是项目符号列表，而是一个菜单。请注意 NavigationTree.hasChildren() 远程调用中的一个参数：async:false。这很有趣，因为它强制对服务器的调用变为同步调用，就像任何给定编程语言中的普通方法调用一样。在 A(同步)JAX 中使用同步调用时必须格外小心——它可能会显著降低应用程序的速度。

下一个函数是 expandElement()。它也会检查子元素，并写入指向菜单或内容的新链接。

第三个函数是 showContent()。它调用远程 Java 方法并检索给定元素的内容。

至此，这个示例菜单树就完成了。尽管菜单是硬编码的，但它为开发由数据库或其他机制支持的动态树菜单提供了一个良好的起点。

## 测试导航树

启动导航树示例时，会看到一个顶层树，如下图所示：

![](img/5b721cfaa97eb697efc92e3914f62fca_122_0.png)

我们可以展开菜单元素，如下面的两个截图所示：先展开**北半球**，再展开**芬兰**。图中会显示**芬兰**的首都，点击它即可获取其描述。

![](img/5b721cfaa97eb697efc92e3914f62fca_123_0.png)

当我们展开**芬兰**并点击**赫尔辛基**时，会得到如下图所示的内容：

![](img/5b721cfaa97eb697efc92e3914f62fca_123_1.png)

## 后记

本示例演示了导航树，以及 DWR 如何用于动态提供菜单元素。

## 使用 DWR 实现地图滚动

本章的最后一个示例是使用 DWR 实现的地图滚动示例。自谷歌地图发布以来，基于浏览器的、由鼠标控制的滚动地图已变得非常流行。

实现本示例的主要思想是：取一张足够大的地图，把它分割成大小相等的小方块。然后，根据位置，将一定数量的小方块插入到可见的地图窗口中；当用户按下鼠标左键并在地图窗口中拖动鼠标来滚动地图时，从服务器检索新的地图方块（其他实现也可能下载可见地图窗口周围的地图方块）。

在本示例中，我们使用了由美国地质调查局天体地质学研究计划（http://astrogeology.usgs.gov/）提供的火星坦佩-马雷奥蒂斯区域地质图。关于地图的直接链接和其他信息，请参阅 http://astrogeology.usgs.gov/Projects/MapBook/fulllisting.jsp?mapNumber=1170。

下面的截图展示了本示例所使用的坦佩-马雷奥蒂斯区域的完整地图。

![](img/5b721cfaa97eb697efc92e3914f62fca_124_0.png)

我们使用两个缩放级别。缩放级别 0 的原始图像尺寸为 1000x825 像素，缩放级别 1 的图像尺寸为 2000x1632。两幅原始图像都被分割成每个 100x100 像素的小方块，但地图底部除外，因为其高度不能被 100 整除。

以下是地图方块的几个随机示例：

![](img/5b721cfaa97eb697efc92e3914f62fca_125_0.png)

由于原始图片相当大，缩放级别 0 共有 90 个地图方块，缩放级别 1 共有 340 个方块。

示例代码演示了如何使用 DWR 和 JavaScript 实现滚动地图应用程序。我们开发的应用程序如下图所示。图像方块带有边框，以便可视化地图的构建方式。

![](img/5b721cfaa97eb697efc92e3914f62fca_125_1.png)

## 开发用户界面

我们从 HTML 文件开始开发。地图滚动器的 HTML 代码如下：

```
<h3>Map Scrolling</h3>
<p>Simple map scroller using geologic map of the Tempe-Mareotis Region of Mars.<br/> Full description from <a href="http://astrogeology.usgs.gov/Projects/MapBook/fullisting.jsp?mapNumber=1170">here</a>.</p>
<div id="mapArea">
Zoom level: <form id="zoomLevelForm"><select id="zoomLevel" onchange="setZoomLevel(this);return false;"><option>0</option><option>1</option></select></form><br/>
<iframe width="430" height="440" scrolling="no" frameborder="0" src="MapArea.html">
</iframe>
</div>
```

用于缩放的控制元素是一个选择框，它使用 JavaScript 函数为地图设置新的缩放级别。setZoomLevel() 函数位于 index.jsp 文件中。函数代码如下：

```
function setZoomLevel(element)
{
    var selectedIndex = element.selectedIndex;
    var value= element.options[selectedIndex].value;
    var frames=window.frames;
    frames[0].setZoomLevel(value);
}
```

该函数接收 select 元素，从中提取一个值，然后调用位于 MapArea.html 文件中的另一个 setZoomLevel() 函数。

地图区域是一个 iframe 元素，其源代码位于 MapArea.html 文件中，如下所示。该文件位于 WebContent 目录下。

```
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<script type='text/javascript' src='/DWREasyAjax/dwr/interface/MapScroller.js'></script>
<script type='text/javascript' src='/DWREasyAjax/dwr/engine.js'></script>
<script type='text/javascript' src='/DWREasyAjax/dwr/util.js'></script>
<script type="text/javascript">
```

```
var zoomLevel = 0;
var mapPositionIndex= 0;

var refGridX=0;
var refGridY=0;

function getRowIndex()
{
    var tempIndex=0;
    if(zoomLevel==0)
    {
        if(mapPositionIndex>=10)
        {
            tempIndex=parseInt(mapPositionIndex.toString().substring(1));
        }
        else
        {
            tempIndex=mapPositionIndex;
        }
    }
    else
    {
        if(mapPositionIndex>=100)
        {
            tempIndex=parseInt(mapPositionIndex.toString().substring(1));
        }
        else
        {
            tempIndex=mapPositionIndex;
        }
        while(tempIndex>16)
        {
            tempIndex-=20;
        }
        if(tempIndex<0)
        {
            tempIndex=16;
        }
    }
    return tempIndex;
}
```

```
function setZoomLevel(newZoomLevel)
{
    if(newZoomLevel==0)
    {
        oldMapPositionIndex=mapPositionIndex;
        newRow=Math.round(Math.round(mapPositionIndex/17.0)/2.0)-1;
        if(newRow>5)
        {
            newRow=5;
        }
        else
        {
            if(newRow>=1 && newRow<=5)
            {
                newRow--;
            }
        }
        newColumn=Math.floor(getRowIndex()/2.0)-1;
        if(newColumn>6)
        {
            newColumn=6;
        }
        mapPositionIndex=(newRow*10)+newColumn;
        if(oldMapPositionIndex==208)
        {
            mapPositionIndex=43;
        }
        if(oldMapPositionIndex==214)
        {
            mapPositionIndex=46;
        }
        if(mapPositionIndex<0)
        {
            mapPositionIndex=0;
        }
    }
    else
    {
        mapPositionIndex=(Math.floor(mapPositionIndex/10.0)*40)+40+(2*(getRowIndex()+1));
    }
    zoomLevel=newZoomLevel;
    getMapImages(zoomLevel,mapPositionIndex);
}
```

