```
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
```