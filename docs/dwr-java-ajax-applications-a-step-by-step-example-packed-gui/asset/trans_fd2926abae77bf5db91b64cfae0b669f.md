

function getMapImages(zoomLevel,mapPositionIndex)
{
    MapScroller.getMapImages(zoomLevel,mapPositionIndex, {
        callback:function(images) {
            setImages(images, zoomLevel);
        }
    });
}

function setRefGrid(event)
{
    refGridX=Math.floor(event.clientX/100.0);
    refGridY=Math.floor(event.clientY/100.0);
}

function startScroll(event)
{
    setRefGrid(event);
    document.addEventListener("mousemove", mouseMove,  true);
    document.addEventListener("mouseup",  stopScroll, true);
    document.addEventListener("mouseout",  stopScrollOnOut, true);
    document.addEventListener("mouseover",  stopScrollOnOut, true);
    event.preventDefault();
}

function stopScroll(event)
{
    document.removeEventListener("mousemove", mouseMove,  true);
    document.removeEventListener("mouseup",  stopScroll, true);
    document.removeEventListener("mouseout",  stopScrollOnOut, true);
    document.removeEventListener("mouseover",  stopScrollOnOut, true);
}

function stopScrollOnOut(event)
{
    var src=event.target;
    if(src.id=="scrollingMapArea")
    {
        stopScroll(event);
    }
}

```javascript
function mouseMove(event)
{
    x=event.clientX;
    y=event.clientY;
    newGridX=Math.floor(x/100.0);
    newGridY=Math.floor(y/100.0);
    deltaX=newGridX-refGridX;
    deltaY=newGridY-refGridY;
    if(deltaX==0 && deltaY==0)
    {
        return;
    }
    var update=false;
    var tilesInRow=10;
    if(zoomLevel==1)
    {
        tilesInRow=20;
    }
    tempIndex=getRowIndex();

    if(deltaX>0)
    {
        if((tempIndex<6 && zoomLevel==0) || (tempIndex<16 && zoomLevel==1))
        {
            mapPositionIndex++;
            update=true;
        }
    }
    else
    {
        if(deltaX<0)
        {
            if(tempIndex>0)
            {
                if(mapPositionIndex-1>=0)
                {
                    mapPositionIndex--;
                    update=true;
                }
            }
        }
    }
}
```

```javascript
if(deltaY>0)
{
    var tempYIndex=50;
    if(zoomLevel==1)
    {
        tempYIndex=260;
    }
    if(mapPositionIndex<tempYIndex)
    {
        mapPositionIndex+=tilesInRow;
        update=true;
    }
}
else
{
    if(deltaY<0)
    {
        if(mapPositionIndex>0 && mapPositionIndex>=tilesInRow)
        {
            mapPositionIndex-=tilesInRow;
            if(mapPositionIndex<0)
            {
                mapPositionIndex=0;
            }
            update=true;
        }
    }
}

if(update)
{
    setRefGrid(event);
    getMapImages(zoomLevel,mapPositionIndex);
}

function setImages(images,zoomLevel)
{
    var mapArea=dwr.util.byId('scrollingMapArea');
    mapArea.innerHTML="";
    for(index in images)
    {
        var imageName=images[index];
        var mapPart = document.createElement("img");
        mapPart.setAttribute('border','1');
        mapPart.setAttribute('src','mapimages/'+imageName);
        mapArea.appendChild(mapPart);
    }
}
```

}
</script>
</head>
<body onMouseDown="startScroll(event)">
<div id="scrollingMapArea"></div>
</body>

<script type="text/javascript">
getMapImages(zoomLevel,mapPositionIndex);
</script>
</html>.

该 HTML 文件包含与地图滚动相关的所有内容。在 body 元素中有一个单一的 div 元素，用于存放地图方块。我们记得 iframe 的宽度和高度为 430x430，而地图方块为 100x100，因此地图视口总共将有 16 张地图图像。

在 JavaScript 代码中，mapPositionIndex 变量是视口左上角的数组索引。例如，当地图显示左上角 mapPositionIndex 为 0 时，并逐行滚动到底部时，mapPositionIndex 会变为 10（缩放级别 0）或 20（缩放级别 1）。

首先，getRowIndex() JavaScript 函数计算可见视口顶部的行。

![](img/5b721cfaa97eb697efc92e3914f62fca_132_0.png)

第二个重要函数是 setZoomLevel()。它计算新缩放级别的 mapPositionIndex。放大时，中心四个方块会被放大显示，缩小 时，可见方块位于中心。上面的截图是缩放对用户可见效果的示例。左图为缩放级别 0，右图为缩放级别 1。

使用 zoomLevel 和 mapPositionIndex，通过 getImages() 函数检索图像。它只是调用远程 Java 方法，其中 Java 方法的回调 函数为 setImages()。

setImages() 函数从 Java 方法接收图像名称数组，并使用 for 循环创建 img 元素，这些元素被追加到地图区域 div 元素中。

setRefGrid()、startScroll()、stopScroll()、stopScrollOnOut() 和 mouseMove() 函数用于处理地图滚动。滚动的工作方式是，当用户按下鼠标按钮时调用 startScroll() 函数。然后该函数为 mousemove、mouseup、mouseout 和 mouseover 事件添加事件监听器。当鼠标按钮按下并移动鼠标时，就会调用 mouseMove() 函数。

mouseMove() 函数计算鼠标所在的网格位置（请记住地图视口为 4x4 方块）。参考位置由 setRefGrid() 函数设置（按钮按下时也会调用）。如果鼠标光标移动到新的网格位置，则使用新的 mapPositionIndex 调用 getImages() 函数，并在页面中更新地图方块。

当释放鼠标按钮时，会调用 stopScroll() 函数，并移除所有事件监听器。当用户将鼠标光标移出地图区域 div 元素时也会发生这种情况。

实际的地图图像位于 WebContent | mapimages 目录中。缩放级别 0 的地图方块图像为 tempe-mareotis-zlevel-0-0.jpg 到 tempe-mareotis-zlevel-0-89.jpg，缩放级别 1 为 tempe-mareotis-zlevel-1-0.jpg 到 tempe-mareotis-zlevel-1-339.jpg，因此仅两个缩放级别就超过 400 张图像。

## 创建 MapScroller Java 类

最后，我们需要返回图像名称的 Java 类。Java 代码如下：

```
package samples;

import java.util.List;
import java.util.Vector;

public class MapScroller {

    private int[] imagesInRowInZLevel = { 10, 20 };
    private String imageName = "tempe-mareotis-zlevel-";
```

```
public List<String> getMapImages(int zoomLevel,
    int mapPositionIndex) {
    List<String> images = new Vector<String>();
    String imageName = this.imageName + zoomLevel + "-";
    for (int rowIndex = 0; rowIndex < 4; rowIndex++) {
        for (int i = 0; i < 4; i++) {
            int index = mapPositionIndex + i;
            images.add(imageName + index + ".jpg");
        }
        mapPositionIndex = mapPositionIndex
            + imagesInRowInZLevel[zoomLevel];
    }
    return images;
}
```

MapScroller 类中只有一个方法 getMapImages()。该方法为 HTML 页面中的当前视图返回 16 个图像名称。参数 mapPositionIndex 是图像的左上角，用于计算其他图像的索引。

不要忘记将远程类添加到 dwr.xml 文件中：

```
<create creator="new" javascript="NavigationTree">
    <param name="class" value="samples.NavigationTree" />
</create>
```

至此，地图滚动示例完成。大多数功能都在浏览器端的 JavaScript 函数中。但如果没有 AJAX，这种功能是不可能实现的。这看起来是一个非常简单的实现，因为 DWR 隐藏了浏览器与服务器之间异步通信的底层细节，并允许我们仅用一个 Java 类中的几行服务器端代码和一堆地图方块图像即可开发地图滚动功能。

## 测试地图滚动器

以下三个截图显示了地图滚动器的运行情况。请注意鼠标光标和手绘箭头，显示了光标的移动。

![](img/5b721cfaa97eb697efc92e3914f62fca_135_0.png)

