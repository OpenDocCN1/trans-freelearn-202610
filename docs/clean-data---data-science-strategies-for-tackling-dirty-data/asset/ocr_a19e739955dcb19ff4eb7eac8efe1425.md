```
d3.csv("fergusonURLcounts.csv", type, function(error, data) {
    x.domain(data.map(function(d) { return d.url; }));
    y.domain([0, d3.max(data, function(d) { return d.num; })]);

    svg.append("g")
        .attr("class", "x axis")
        .attr("transform", "translate(0," + height + ")")
        .call(xAxis);

    svg.append("g")
        .attr("class", "y axis")
        .call(yAxis)
        .append("text")
        .attr("transform", "rotate(-90)")
        .attr("y", 6)
        .attr("dy", "-3em")
        .style("text-anchor", "end")
        .text("Frequency");

    svg.selectAll(".bar")
        .data(data)
        .enter().append("rect")
        .attr("class", "bar")
        .attr("x", function(d) { return x(d.url) ; })
        .attr("width", x.rangeBand())
        .attr("y", function(d) { return y(d.num); })
        .attr("height", function(d) { return height - y(d.num); });
});

function type(d) {
    d.num = +d.num;
    return d;
}

</script>
</body>
</html>
```