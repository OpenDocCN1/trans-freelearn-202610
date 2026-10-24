```
```javascript
.append("div")
.attr("id","loader")
.style("top",d3.select("#play").node().offsetTop + "px")
.style("height",d3.select("#date").node().offsetHeight +
    d3.select("#map-container").node().offsetHeight + "px");

//load and parse whites.csv
d3.csv("csv/whites.csv",function(data){
  var first = data[0];
  // get columns
  for ( var mug in first ){
    if ( mug != "name" && mug != "lat" && mug != "lon" ){
      orderedColumns.push(mug);
    }
  }

  orderedColumns.sort( sortColumns );

  // draw city points
  for ( var i in data ){
    var projected = projection([ parseFloat(data[i].lon),
      parseFloat(data[i].lat) ])
    map.append("circle")
      .datum( data[i] )
      .attr("cx",projected[0])
      .attr("cy",projected[1])
      .attr("r",1)
      .attr("vector-effect","non-scaling-stroke")
      .on("mouseover",function(d){
        hoverData = d;
        setProbeContent(d);
        probe
          .style({
            "display" : "block",
            "top" : (d3.event.pageY - 80) + "px",
            "left" : (d3.event.pageX + 10) + "px"
          })
      })
      .on("mouseout",function(){
        hoverData = null;
        probe.style("display","none");
      })
  }
})
```
```