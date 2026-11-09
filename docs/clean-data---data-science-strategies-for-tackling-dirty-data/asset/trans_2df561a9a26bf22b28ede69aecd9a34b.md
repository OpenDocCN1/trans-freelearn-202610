
目前我们还没有 JSON 文件。我们只有一份 GDF 文件。我们将如何构建这个 JSON 文件？仔细查看 GDF 文件后，可以发现它看起来很像两份 CSV 文件叠放在一起。从本章前面部分我们知道，有几种不同的策略可以将 CSV 转换为 JSON。

因此，我们决定先将 GDF 转换为 CSV，再将 CSV 转换为 JSON。

![](img/9aa003afabe05fdb07da46ad71325de6_118_0.png)

> 等等；如果那个 JSON 示例看起来不像我在网上找到的用于在 D3 中绘制社交网络图的 JSON 文件怎么办？
> 你在网上找到的一些 D3 社交网络可视化示例会为每个节点或链接显示许多额外的值，例如，它们可能包含可用于表示大小差异、悬停功能或颜色变化的额外属性，如此示例所示： http://bl.ocks.org/christophermanning/1625629。
> 此可视化显示了芝加哥付费政治游说者的关系。在此示例中，代码会考虑 JSON 文件中的信息来确定节点的圆圈大小以及悬停在节点上时显示的文本。它制作出非常漂亮的图表，但很复杂。由于我们的主要目标是学习如何清理数据，我们将在此使用一个精简、简单的示例，其中没有太多这些额外内容。别担心；我们的示例仍然会构建一个不错的 D3 图表！

将 GDF 文件转换为我们想要的格式的 JSON 时，可以按照以下步骤操作：

- 1. 使用文本编辑器将 `personal.gdf` 文件拆分为两个文件，`nodes.gdf` 和 `links.gdf`。
- 2. 修改每个文件中的标题行，使其与我们最终希望在 JSON 文件中使用的列名相匹配：
```
id,name,gender,lang,num
1234,Bugs Bunny,male,en_US,296
2345,Daffy Duck,male,en_US,295
9876,Minnie Mouse,female,en_US,294

source,target
1234,2345
1234,9876
2345,9876
```
- 3. 使用 `csvcut` 工具（之前讨论过的 csvkit 的一部分）从 `nodes.gdf` 文件中提取第一列和第二列，并将输出重定向到名为 `nodesCut.gdf` 的新文件：
```
csvcut -c 1,2 nodes.gdf > nodesCut.gdf
```

- 4. 现在，我们需要为每条边对赋予索引值，而不是其完整的 Facebook ID 值。索引只是通过其在节点列表中的位置来标识该节点。我们需要执行此转换，以便数据能够轻松地输入到我们现有的 D3 力网络代码示例中，且尽可能少地进行重构。我们需要将此内容转换为：
```
source,target
1234,2345
1234,9876
2345,9876
```
into this:
```
source,target
0,1
0,2
1,2
```
下面是一个小型 Python 脚本，可自动创建这些索引值：
```
import csv

# read in the nodes
with open('nodesCut.gdf', 'r') as nodefile:
    nodereader = csv.reader(nodefile)
    nodeid, name = zip(*nodereader)

# read in the source and target of the edges
with open('edges.gdf', 'r') as edgefile:
    edgereader = csv.reader(edgefile)
    sourcearray, targetarray = zip(*edgereader)
slist = list(sourcearray)
tlist = list(targetarray)

# find the node index value for each source and target
for n,i in enumerate(nodeid):
    for j,s in enumerate(slist):
        if s == i:
            slist[j]=n-1
    for k,t in enumerate(tlist):
        if t == i:
            tlist[k]=n-1
# write out the new edge list with index values
with open('edgelistIndex.csv', 'wb') as indexfile:
    iwriter = csv.writer(indexfile)
    for c in range(len(slist)):
        iwriter.writerow([ slist[c], tlist[c]])
```

- 5. 现在，回到 nodesCut.csv 文件并删除 id 列：
```
csvcut -c 2 nodesCut.gdf > nodesCutName.gdf
```

- 6. 编写一个小型 Python 脚本，将这些文件读取并写入完整的 JSON 文件，以便 D3 处理：
```
import csv
import json

# read in the nodes file
with open('nodesCutName.gdf') as nodefile:
    nodefile_csv = csv.DictReader(nodefile)
    noutput = '['
    ncounter = 0;

    # process each dictionary row
    for nrow in nodefile_csv:
        # look for ' in node names, like O'Connor
        nrow["name"] = \
            str(nrow["name"]).replace("'",",")
        # put a comma between the entities
        if ncounter > 0:
            noutput += ','
        noutput += json.dumps(nrow)
        ncounter += 1
    noutput += ']'
    # write out a new file to disk
    f = open('complete.json','w')
    f.write('{')
    f.write('\"nodes\": ')
    f.write(noutput)

# read in the edge file
with open('edgelistIndex.csv') as edgefile:
    edgefile_csv = csv.DictReader(edgefile)
    eoutput = '['
    ecounter = 0;
    # process each dictionary row
    for erow in edgefile_csv:
        # make sure numeric data is coded as number not
        # string
        for ekey in erow:
            try:
                erow[ekey] = int(erow[ekey])
```
```
except ValueError:
    # not an int
    pass
# put a comma between the entities
if ecounter > 0:
    eoutput += ','
eoutput += json.dumps(erow)
    ecounter += 1
eoutput += ']'

# write out a new file to disk
f.write(',')
f.write('"links":')
f.write(eoutput)
f.write('}')
f.close()
```

## 第四步 – 构建 D3 图表

本节展示如何将我们的节点和链接 JSON 文件输入到 D3 中构建力导向图的样板示例中。此代码示例来自 D3 网站，并使用提供的 JSON 文件构建一个简单的图。每个节点显示为一个圆圈，当将鼠标悬停在节点上时，该人的姓名会作为工具提示显示：
```
<!DOCTYPE html>
<!-- this code is based on the force-directed graph D3 example given
at : https://gist.github.com/mbostock/4062045 -->

<meta charset="utf-8">
<style>

.node {
    stroke: #fff;
    stroke-width: 1.5px;
}

.link {
    stroke: #999;
    stroke-opacity: .6;
}

</style>
```
```
<body>
<!-- make sure you have downloaded the D3 libraries and stored them locally -->
<script src="d3.min.js"></script>
<script>

var width = 960, height = 500;
var color = d3.scale.category20();
var force = d3.layout.force()
    .charge(-25)
    .linkDistance(30)
    .size([width, height]);

var svg = d3.select("body").append("svg")
    .attr("width", width)
    .attr("height", height);

d3.json("complete.json", function(error, graph) {
    force
        .nodes(graph.nodes)
        .links(graph.links)
        .start();

    var link = svg.selectAll(".link")
        .data(graph.links)
        .enter().append("line")
        .attr("class", "link")
        .style("stroke-width", function(d) { return Math.sqrt(d.value);
});

var node = svg.selectAll(".node")
    .data(graph.nodes)
    .enter().append("circle")
    .attr("class", "node")
    .attr("r", 5)
    .style("fill", function(d) { return color(d.group); })
    .call(force.drag);

node.append("title")
    .text(function(d) { return d.name; });

force.on("tick", function() {
    link.attr("x1", function(d) { return d.source.x; })
        .attr("y1", function(d) { return d.source.y; })
```
```
.attr("x2", function(d) { return d.target.x; })
.attr("y2", function(d) { return d.target.y; });

node.attr("cx", function(d) { return d.x; })
.attr("cy", function(d) { return d.y; });
});
});
</script>
```

以下屏幕截图显示了此社交网络的一个示例。其中一个节点已被悬停，显示了该节点的工具提示（名称）。

![](img/9aa003afabe05fdb07da46ad71325de6_123_0.png)

使用 D3 构建的社交网络

## 第五步 – 将数据转换为 Pajek 文件格式

到目前为止，我们已将 GDF 文件转换为 CSV，然后转换为 JSON，并为其构建了 D3 图表。在接下来的两个步骤中，我们将继续追求我们的目标，即将数据置于可以对其计算一些社交网络指标的格式中。

对于此步骤，我们将采用原始的 GDF 文件并对其进行调整，使其成为有效的 Pajek 文件，这是社交网络工具 networkx 所需的格式。
