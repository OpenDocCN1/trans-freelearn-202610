

![](img/71ff737206f7d80bb625b3c9aca9dc7f_145_0.png)

图7.3 – 打印机耗材仓库示意图

然而，4号仓库服务于市中心区域，而由于喜欢通勤便利的年轻家庭纷纷迁往郊区，该地区人口大幅流失。返岗政策并未吸引员工回到市中心的公寓，导致大多数建筑半空置或由季节性居民居住。理想情况下，我们希望避免出现供应品堆积却没有潜在客户的情况（如图7.4所示）：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_145_1.png)

图7.4 – 一家无人光顾的废弃打印机耗材店示意图

我们的目标是及时发现趋势，以便尽快调整业务战略，满足区域日益增长的耗材需求，并在都市区租金飙升的同时尽量减少房地产开支。鉴于这些仓库位于同一地理区域，我们将假设空间权重矩阵将四家仓库相互连接。在更大的连锁门店中，基于人口特征或与其他门店的距离来创建空间权重，可能会使分析更加精确。

现在，让我们将注意力转向代码，它将帮助我们深入分析布基纳法索的市场数据和COVID-19门店销售数据。

## 分析时空数据集

我们的布基纳法索市场数据集已按季度划分，为理解逐季变化，合理做法是将窗口大小设置为四个季度（一年），并重叠三个季度（以关注月度变化）。正如我们在第2章所做的那样，我们将通过权重矩阵和相关性指标计算局部莫兰统计量。让我们使用脚本7.1设置所需包，并导入布基纳法索市场数据集及其权重矩阵：

```
#import packages
import igraph as ig
from igraph import Graph
import numpy as np
import pandas as pd
import os
import matplotlib.pyplot as plt

#import Burkina Faso market millet prices
File="C:/users/njfar/OneDrive/Desktop/BF_Millet.csv"
pwd=os.getcwd()
os.chdir(os.path.dirname(File))
mydata=pd.read_csv(os.path.basename(File),encoding='latin1')

#import weight matrix of Burkina Faso markets
File="C:/users/njfar/OneDrive/Desktop/weights_bk.csv"
pwd=os.getcwd()
os.chdir(os.path.dirname(File))
weights=pd.read_csv(os.path.basename(File),encoding='latin1')
```

现在我们已经有了数据和包，让我们计算Forman-Ricci曲率和PageRank中心性，并通过在脚本7.1中添加相关阈值设为0.9的代码（以对重大变化保持最大敏感度），保存每个时间片的网络和整体指标：

```
#score in yearly sets with 3 quarter overlap
vcurv_t=[]
vcurv_ave=[]
```

```
eig_t=[]
eig_ave=[]
nets=[]

for Year in range(4,29):
    data=mydata.iloc[(Year-4):(Year),1:46]
    cor=np.corrcoef(data.transpose())
    weights_total=weights.iloc[:,1:46]
    cor[cor>=0.9]=1
    cor[cor<0.9]=0
    cor_weighted=np.multiply(cor,weights_total)
    bf_market_w=Graph.Adjacency(cor_weighted,diag=False)
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
    nets.append(bf_market_w)
    d=Graph.degree(bf_market_w)
    e=Graph.pagerank(bf_market_w)
    eig_t.append(e)
    eig_ave.append(np.mean(e))
    ecurvw=[]

    for edge in bf_market_w.es:
        s=edge.source
        t=edge.target
        ecurvw.append(2-d[s]-d[t])
    vcurvw=[]

    for vertex in bf_market_w.vs:
        inc=Graph.incident(bf_market_w,vertex)
        inc_curv=[]
        for I in inc:
```

```
inc_curv.append(ecurvw[i])
vcurvw.append(sum(inc_curv))
vcurv_t.append(vcurvw)
vcurv_ave.append(np.mean(vcurv_t))
```

既然我们已经计算出了指标，让我们通过向脚本7.1添加以下代码来绘制结果，以观察PageRank和Forman-Ricci曲率中心性如何随时间变化：

```
#plot metric averages across time slices
time=range(0,25)
plt.plot(time, eig_ave, label = "PageRank Average")
plt.plot(time, vcurv_ave, label = "Forman-Ricci Curvature Average")
plt.xlabel("Time Slice")
plt.ylabel("PageRank and Forman-Ricci Curvature Averages")
plt.legend()
plt.show()
```

这应会显示一个图表，其中PageRank中心性的变化很小，而Forman-Ricci曲率中心性则出现上升和周期性波动（如图7.5所示）。这表明，我们各市场的小米价格波动性有所增加，大约从2018-2019年间开始（波动性先是上升，然后下降，再次上升，最后在更高的波动水平上趋于平稳），并在COVID-19和乌克兰冲突期间略有恶化。

![](img/71ff737206f7d80bb625b3c9aca9dc7f_148_0.png)

图7.5 – 2015-2022年布基纳法索市场小米价格的平均中心性指标图

现在我们有了显示波动性变化的图，我们可以考察不同时期的市场状况。你可能希望研究所有时期，但我们将比较两个时期——一个低波动期和一个高波动期——并在以下代码中展示。让我们绘制第4个时间段，根据Forman-Ricci曲率中心性，该时期波动性较低（数据集中有最高的中心性测量值），每个顶点的大小按该顶点的Forman-Ricci曲率加权，只需在脚本7.1中添加以下代码：

```
#examine different time points with Forman-Ricci
#curvature plots, fourth slice
ig.plot(nets[4],vertex_size=np.array(vcurv_t[4])*-0.5)
```

这应会生成类似图7.6的图，显示通过相关性和空间加权连接的市场很少，且顶点的Forman-Ricci曲率非常低（顶点尺寸非常小）：

![](img/71ff737206f7d80bb625b3c9aca9dc7f_149_0.png)

图7.6 – 显示同一地理区域内相关性 >0.9 且按Forman-Ricci曲率中心性加权的市场连接图

图7.6显示，大多数市场彼此并不相连，而那些彼此相连的市场之间的相互连接程度也往往很低，因此Forman-Ricci曲率中心性指标接近0。这意味着价格是独立的，表明市场网络具有稳健性，不太可能受到COVID-19或国际贸易变化等外部因素的严重影响。影响一两个市场的变化不太可能影响全国所有市场，受到局部问题（如干旱季度）影响的卖家和消费者可以在需要时从邻近地区寻求粮食。政府也可以通过简单地将粮食从供应充足的省份调往存在供应问题的省份来解决短缺问题。从政府的角度来看，解决局部供应问题不会花费太多时间和资金。

让我们考察一个高波动性时期（第19期），并通过向脚本7.1添加以下代码，看看连通性和Forman-Ricci曲率中心性与第4期相比如何：

```
```python
#examine different time points with Forman-Ricci
#curvature plots, 19th slice
ig.plot(nets[19],vertex_size=np.array(vcurv_t[19])*-0.5)
```
```

这应会生成类似图7.7的图，其中许多市场相互连接，使得Forman-Ricci曲率中心性指标远偏离零，表明全国大范围地区的供应链高度互联，容易受到可能导致该地区饥荒的严重短缺的影响。解决这些问题将使政府花费更多的时间和成本，使人们面临粮食短缺或价格飙升的风险，消费者可能买不起谷物。

> **注意**
> 
> 一些相连的市场共享许多相互连接，产生较强的曲率指标，而一些市场则仍未与其他任何市场相连。连接度较低的市场价格表明，某些地区不太容易受到可能导致饥荒的广泛粮食价格飙升或短缺的影响。

![](img/71ff737206f7d80bb625b3c9aca9dc7f_150_0.png)

图7.7 – 布基纳法索市场小米价格网络中的第19个时间段

