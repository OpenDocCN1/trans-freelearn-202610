

# 联邦学习（隐私保护深度学习）用于分布式图像分类

传统的深度学习假设所有训练数据都收集并存储在中央服务器上。在许多现实世界应用中，例如医院、移动设备、银行和自动驾驶汽车，由于隐私、法律或安全限制，共享原始数据可能是不可能的。

联邦学习（FL）通过允许多个客户端在不共享本地数据的情况下协作训练模型来解决这一挑战。每个客户端不是传输图像，而是训练本地模型并仅将模型更新（权重或梯度）发送到中央服务器。

Server ← Model Updates ← Clients

服务器聚合更新并将改进后的全局模型分发回所有客户端。

数学表述：假设有 K 个客户端。客户端 k 拥有本地数据集 D_k = (x_i, y_i) 并最小化其本地目标：

F_k(w) = 1/|D_k| Σ_{(x,y)∈D_k} L(f(x; w), y)

其中 w = 网络参数，L = 分类损失，f = CNN 模型。

全局优化问题变为：

F(w) = Σ_{k=1}^K |D_k|/N F_k(w)

其中 N = Σ_{k=1}^K |D_k| 是样本总数。

联邦平均（FedAvg）：最广泛使用的联邦学习算法是联邦平均（FedAvg）。每个客户端本地训练并产生权重 w_1, w_2, ..., w_K。服务器计算 w_global = 1/K Σ_{k=1}^K w_k 并重新分发平均模型。这种简单的平均过程使所有客户端都能从集体学习中受益，同时保持其数据私密。

想象五个医院（A、B、C、D、E）各自拥有医学图像。医院 A、B 和 C 从不共享图像。相反，每个医院训练一个本地 CNN 并仅将学习到的参数发送到中央服务器。服务器平均参数并创建一个更强的全局模型，该模型在从未看到其原始数据的情况下间接从所有医院学习。

数据集：Fashion-MNIST：演示使用手写时尚数据集：28 × 28 灰度服装图像，属于十个类别：T-Shirt、Trouser、Pullover、Dress、Coat、Sandal、Shirt、Sneaker、Bag 和 Ankle Boot。数据集随机划分为五个模拟客户端。

```
num_clients = 5
client_indices = np.array_split(indices, num_clients)
```

每个客户端接收其自己的私有子集。

## CNN 分类器
使用轻量级 CNN 进行图像分类。

```
class CNN(nn.Module):
    def forward(self,x):
        f = self.features(x)
        return self.classifier(f)
```

架构由卷积层用于特征提取、最大池化用于空间缩减以及全连接层用于分类组成。

## 本地客户端训练
每个客户端使用其本地数据集独立训练。

```
def local_train(model, loader):
    pred = model(x)
    loss = criterion(pred, y)
    loss.backward()
```

客户端永远不会将其图像暴露给任何其他客户端或服务器。仅共享学习到的权重。

## 联邦聚合
本地训练后，服务器使用 FedAvg 聚合客户端模型。

```
def fedavg(weights):
    avg = copy.deepcopy(weights[0])
    for key in avg.keys():
        for i in range(1, len(weights)):
            avg[key] += weights[i][key]
        avg[key] /= len(weights)
    return avg
```

概念上，

$w_{global} = \frac{1}{K} \sum_{k=1}^{K} w_k$

 resulting model becomes the new global model.

## 联邦训练循环
训练通过多次通信轮次进行。

```
for r in range(rounds):
    local_weights = []
    for client_loader in clients:
        w = local_train(local_model, client_loader)
    global_model.load_state_dict(fedavg(local_weights))
```

每个轮次包括
- 将全局模型发送给客户端
- 本地客户端训练
- 聚合客户端模型
- 更新全局模型

该过程重复进行直到收敛。

## 集中式学习基线
为了比较，使用整个数据集训练传统的集中式模型。

```
central_model = CNN()
```

这代表了所有数据可以存储在一个位置的理想场景。

## 联邦与集中式准确率
实验比较了两种方法获得的测试准确率。如下一图所示，联邦和集中式分类器的准确率随训练轮次增加，联邦准确率最终达到 90.6%（略低于其集中式对应物）。

```
Plt.plot(federated_acc)
Plt.plot(central_acc)
```

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_639_0.png)

*Figure 12.25 – Federated vs. Centralized classifier performance over epochs*

### 观察到的性能：

| Round | Federated Accuracy |
|-------|--------------------|
| 0 | 77.9% |
| 5 | 89.3% |
| 9 | 90.6% |

*Table 12.7: Federated classification accuracy over rounds*

## Note
由于联邦学习在异构本地数据上优化模型且通信受限，与集中式训练相比会失去一些全局统计效率，但仍然接近，因为所有客户端共同近似相同的底层数据分布。联邦模型在从未访问集中式数据的情况下逐渐接近集中式性能。

## 混淆矩阵分析
分类性能可以使用混淆矩阵进行分析。

```
cm = confusion_matrix(y_true, y_pred)
```

矩阵揭示了联邦训练中经常混淆的服装类别、强类、弱类以及错误模式。接近对角线的矩阵表示良好的分类性能。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_640_0.png)

*Figure 12.26 – Confusion matrix for classification*

## 学习特征空间可视化
提取中间 CNN 特征并使用 t-SNE 投影到二维。

```
f = global_model.features(x)
embedding = tsne.fit_transform(features[:3000])
```

 resulting visualization shows how the network organizes clothing categories in feature space. Well-separated clusters indicate that the federated model has learned discriminative representations.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_641_0.png)

*Figure 12.27 – 2D t-SNE manifold of fashion-MNIST dataset with federated learning*

## 客户端数据分布
以下可视化说明了训练样本如何在客户端之间分布。

```
client_sizes = [len(idx) for idx in client_indices]
```

在真实联邦系统中，客户端通常拥有高度不平衡的数据集，使得学习变得更加具有挑战性。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_641_1.png)

*Figure 12.28 – Federated client data distribution*

## 预测可视化
最后一幅图显示了 Fashion-MNIST 测试图像以及预测标签。

```
pred = global_model(img.unsqueeze(0).to(device)).argmax()
```

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_642_0.png)

## 什么是对抗样本？
设 $x$ 表示输入图像，$f(x)$ 为神经网络的预测。对抗图像构造为：

$$x_{adv}=x+\delta$$

其中 $\delta$ 是专门选择以最大化模型分类错误的小扰动。尽管 $|\delta|$ 极小，但 resulting prediction may change dramatically.

### 为什么对抗攻击有效？
神经网络在非常高维空间中运行。即使在数千个像素上应用的微小扰动也可能累积成网络决策边界的巨大变化。直观地，想象站在悬崖边缘。轻微的推动可能足以使你越过边界。同样，图像可能靠近分类边界，允许微小扰动改变预测类别。

### 白盒与黑盒攻击
- **White-Box Attack:** 攻击者知道模型架构、参数和梯度。FGSM 是经典的白盒攻击，因为它直接使用模型梯度。
- **Black-Box Attack:** 攻击者仅观察输出或概率，并且无法访问内部模型细节。这种攻击在实践中通常更现实。

## 快速梯度符号法（FGSM）
最简单且最具影响力的对抗攻击之一是 Ian Goodfellow 等人提出的 **快速梯度符号法（FGSM）**。FGSM 使用分类损失相对于输入图像的梯度。给定损失 $J(\theta,x,y)$，对抗图像为：

$$x_{adv}=x+\epsilon\,\operatorname{sign}(\nabla_xJ(\theta,x,y))$$

其中 $x$ = 原始图像，$y$ = 真实标签，$\theta$ = 模型参数，$\epsilon$ = 攻击强度，$\nabla_xJ$ = 相对于图像像素的损失梯度。sign 操作强制每个像素扰动朝最大增加分类错误的方向移动。

梯度告诉我们：“每个像素应朝哪个方向移动以最快增加网络的损失？” 而不是执行多次优化迭代，FGSM 在该方向上进行单步。扰动因此变为 $\delta=\epsilon\,\operatorname{sign}(\nabla_xJ)$，计算成本低廉但非常有效。

**加载预训练图像分类器：** 演示使用预训练的 MobileNetV2 分类器。

```
```python
model = models.mobilenet_v2(weights=models.MobileNet_V2_Weights.DEFAULT)
model.eval()
```
```

该网络在 ImageNet 数据集上训练，可以识别 1000 个物体类别。

