
Let’s dive into our example dataset, derived from Humanitarian Data Exchange’s Burkina Faso – Food Prices dataset from the World Food Programme Price Database (https://data.humdata.org/dataset/wfp-food-prices-for-burkina-faso; accessed July 15, 2022). In this dataset, we have pre-selected millet prices across 45 Burkina Faso province markets and aggregated data by quarter, starting in Quarter 2 of 2015 and ending in Quarter 2 of 2022. This period covers the COVID-19 pandemic and the beginning of the Ukraine War, both of which influenced supply chains. Given that markets in neighboring provinces may be impacted by the same local factors and supply chains, we have created a spatial weight matrix of 1’s and 0’s, representing adjacent provinces and non-adjacent provinces respectively.

To run all of the code, it may be necessary to install other packages that are dependencies of igraph or NetworkX, such as pycairo for visualizations. Each machine may function differently or have other packages already installed if you are using a cloud platform; if you run into any difficulties, please consult the igraph or NetworkX help guides’ suggestions for running the packages on your operating system.

Let’s first import the needed packages to construct our network in igraph with Script 2.1:

```
```python
#import packages
import igraph as ig
from igraph import Graph
import numpy as np
import pandas as pd
import os
```
```

When importing data, you’ll need to specify your own file path after you download the dataset. We import both the pricing data and the weight matrix from a local machine by adding to Script 2.1:

```
```python
#import Burkina Faso market millet prices from the csv file
File="C:/users/njfar/OneDrive/Desktop/BF_Millet.csv"
pwd=os.getcwd()
os.chdir(os.path.dirname(File))
mydata = pd.read_csv(os.path.basename(File),encoding='latin1')

#import weight matrix of Burkina Faso markets
File="C:/users/njfar/OneDrive/Desktop/weights_bk.csv"
pwd=os.getcwd()
os.chdir(os.path.dirname(File))
weights = pd.read_csv(os.path.basename(File),encoding='latin1')
```
```

Now that we have our data imported, we can construct our local Moran statistic by computing the correlation coefficient across markets (ignoring the time indicator column) and multiplying the result by the weight matrix:

```
#define the metric between markets and construct the local Moran
#statistic
#here, correlation coefficient
data=mydata.iloc[:,1:46]
weights_total=weights.iloc[:,1:46]
cor=np.corrcoef(data.transpose())
cor[cor>0]=1
cor_weighted=np.multiply(cor,weights_total)
```

Now that we have our local Moran statistic constructed as cor_weighted, we can turn our data into a network with igraph. Our graph does not have any directionality of relationship, so we can set the mode to undirected. However, because our correlations include autocorrelations within individual markets, we need to remove loops from our network. We'll do this by defining possible self-loops and removing them from our edge list:

```
# create market graph and get rid of loops created from the correlation
bf_market_w=Graph.Adjacency(cor_weighted,mode="undirected")
edge_list=bf_market_w.get_edgelist()
self_loop=[]
for i in range(0,46):
    self=(i,i)
    self_loop.append(self)
to_remove=[]
for i in edge_list:
    for j in self_loop:
        if i==j:
            to_remove.append(i)
bf_market_w.delete_edges(to_remove)
```

Now, we can visualize our network to see which markets are isolated by plotting our results:

```
#create plot
ig.plot(bf_market_w)
```

## 第四章 系统权限那些事儿

权限对于Linux系统来说，是控制用户能否对文件或目录进行操作的依据。每个文件或目录都涉及三类权限：属主权限、属组权限和其他用户权限。可以使用`ls -l`查看文件详细信息，第一列就是文件的类型和权限信息。

```
```bash
$ ls -l
total 0
-rw-r--r-- 1 root root 0 Apr 10 10:30 file1
```
```

第一列由10个字符组成，第一个字符表示文件类型，后面的9个字符分为三组，分别对应属主权限、属组权限和其他用户权限。`r`表示可读，`w`表示可写，`x`表示可执行，`-`表示没有对应权限。

- 文件类型：`-`表示普通文件，`d`表示目录，`l`表示符号链接
- **r**：可读权限，例如可以查看文件内容
- **w**：可写权限，例如可以修改文件内容
- **x**：可执行权限，例如可以运行该文件

可以使用`chmod`命令修改文件或目录的权限。权限的表示有符号模式（如`u+x`）和数字模式（如`755`）两种。下面是一些常见的示例：

```
```bash
$ chmod u+x file.sh    # 给属主添加执行权限
$ chmod g-w file.txt   # 去掉属组的写权限
$ chmod o=r file.txt   # 其他用户权限设置为只读
$ chmod 755 script.sh  # 属主rwx，属组r-x，其他r-x
```
```

除了权限管理，还可以使用`chown`命令修改文件的所有者，使用`chgrp`命令修改文件的所属组。日常运维中，合理地设置文件和目录的权限，是保障系统安全的重要一环。

| 命令 | 作用 |
| --- | --- |
| `chmod` | 修改文件权限 |
| `chown` | 修改文件属主 |
| `chgrp` | 修改文件所属组 |
| `umask` | 设置默认权限掩码 |

> > 提示：使用`su`或`sudo`切换用户身份时，要仔细确认操作对象，避免因为权限误操作导致系统文件损坏。

本节介绍了Linux系统权限的基本概念和常用命令。下一节将介绍用户与用户组的管理，帮助大家更全面地理解多用户环境下的权限控制方式。

```
```python
# create market graph and get rid of loops created from the
#correlation
bf_market_w=Graph.Adjacency(cor_weighted,mode="undirected")
edge_list=bf_market_w.get_edgelist()
self_loop=[]
for i in range(0,46):
    self=(i,i)
    self_loop.append(self)
to_remove=[]
for i in edge_list:
    for j in self_loop:
        if i==j:
            to_remove.append(i)
bf_market_w.delete_edges(to_remove)

#create plot
ig.plot(bf_market_w)
```
```

This should yield many more isolated markets that are not highly correlated over our time period:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_56_0.png)

Figure 2.10 – A plot of Burkina Faso markets connected by local
Moran statistics over the selected threshold of 0.9

Figure 2.10 shows a much sparser network, where many markets are isolated. Some markets that were regionally connected still show connections among markets; however, many are not mutually connected anymore. Interestingly, our group of six regional markets is still connected with most connections intact after the thresholding. Indeed, only one market in that region shows a difference in connectivity relative to our non-thresholded version.

While this example considers correlations across the entire time period, it is possible to slice spatiotemporal data into overlapping time slices, create time-aware metrics like the local Moran statistic (with or without thresholding), and visualize the metrics’ changes over time by plotting a series of networks created from these time slices and their network-defining metrics. We’ll explore this notion further in Chapter 7 when we break our Burkina Faso millet dataset into time slices to analyze changing graph metrics over time and space.

Now that we know how to wrangle data into an igraph network, let’s see how this is done in NetworkX with another example.

## Social network examples with NetworkX

Here, we’ll consider a social network of African Institute of Mathematical Sciences (AIMS) students collected from a 2022 cohort in Cameroon. Students in this program live together in the same building, interact during class, eat meals together, and study together. However, students who share a country, come from the same undergraduate university, and speak the same language tend to interact more frequently than those who come from different backgrounds. Interactions such as those shown in Figure 2.11 thus happen either organically as a function of background or artificially through shared programs and living arrangements:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_57_0.png)

Figure 2.11 – An illustration of a classroom setting, where students share courses within a program

To understand these interactions within our cohort of students, we administered a questionnaire about interactions with other students in various settings. The data was collected between November and December of 2022. The following are some example questions:

- Whom do you like to sit with during lunch or dinner?
- Who do you like to ask questions when you are having difficulty with a subject or course?

From these questions, students’ perceptions of their networks could be examined to see the degree to which they agree. Upon looking at the data, we realized most interactions were reciprocated. If Student A endorsed survey items of closeness to Student B, Student B was likely to endorse those items, as well. Thus, the data form an undirected network of mutual interactions.

The dataset we’ll use includes several fields. In the first column, we have the name of the surveyed student. In the next four fields, we have demographic factors (to which we’ll return in later chapters), including country of origin, age, field of study, and undergraduate background. In the remaining five columns, we have five friends that the student most strongly endorsed.

Let’s start by importing the packages we’ll need to wrangle this dataset into a network with Script 2.2:

```
#import needed packages
import pandas as pd
import networkx as nx
import matplotlib.pyplot as plt
import numpy as np
import math
```

Now, let’s import the dataset:

```
#reading the dataset
fichier = pd.read_csv("C:/users/njfar/OneDrive/Desktop/AIMS_data.csv")
data = pd.DataFrame(fichier)
```
