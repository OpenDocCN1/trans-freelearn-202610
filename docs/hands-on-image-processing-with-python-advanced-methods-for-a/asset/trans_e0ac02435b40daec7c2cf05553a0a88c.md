

```
normalization=dcrf.NORMALIZE_SYMMETRIC)
Q = d.inference(5) # Run five inference steps
MAP = np.argmax(Q, axis=0) # most probable class for each pixel
MAP = colorize[MAP,:]
MAP = MAP.reshape(img.shape)
# Plot img, anno_rgb, MAP and overlay side-by-side here
```

函数 predict_segmenter() 计算 ŷᵢ = classifier(fᵢ)。生成完整的分割图（逐像素预测）。函数 mark_boundaries() 在图像上叠加边界。相同的模型应用于未见过的数据（泛化到新图像），如下一段代码所示。

```
img_new = full_img[:700, 900:]
features_new = features_func(img_new)
result_new = future.predict_segmenter(features_new, clf)
# Plot img_new with segmentation boundaries and result_new side-by-side
```

如果运行上述代码并将图像并排绘制，你应该得到下图：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_514_0.png)

图 10.15 – 图像、掩码、分割边界以及预测的分割标签（测试数据）

该方法从数据中学习而不是规则。它在手工制作的特征上效果良好。它是深度学习分割（CNNs）的前身。

## 集成分割

组合多个分割输出以提高鲁棒性：

S = S₁ ∩ S₂  或  S₁ ∪ S₂。

不同方法捕捉不同的属性：基于边缘 → 边界，基于区域 → 同质性，而超像素 → 结构。组合它们可以减少错误。下一段代码片段演示了如何在 Python 中实现集成分割。步骤如下：

- 1. 预处理：将输入图像转换为灰度
- 2. 基于边缘的分割（Watershed）：使用 skimage.filters 模块中的函数 sobel() 计算边缘并突出显示边界。定义种子，运行 watershed 并生成分割 S₁
- 3. **超像素分割（SLIC + Graph Cut）：** 使用 `skimage.segmentation` 中的函数 `slic()` 首先生成超像素，然后构建 RAG 并使用 *normalized cut* 最小化：`cut(A,B)/assoc(A,V)`，以生成分割 S₂。
- 4. **组合分割：** 使用函数 `join_segmentations()` 计算 S = S₁ ∩ S₂，它保留方法之间的一致性，如下一段代码片段所示：

```
from skimage.segmentation import slic, join_segmentations, watershed
coins = (255*rgb2gray(data.astronaut())).astype('uint8')
edges, markers = sobel(coins), np.zeros_like(coins)
foreground, background = 1, 2
markers[coins < 30.0] = background
markers[coins > 150.0] = foreground
ws = watershed(edges, markers)
seg1 = label(ws == foreground)
seg2 = slic(coins, n_segments=200, max_num_iter=160, \
            sigma=1, compactness=0.75, channel_axis=None, start_label=0)
g = graph.rag_mean_color(coins, seg2, mode='similarity')
seg2 = graph.cut_normalized(seg2, g)
segj = join_segmentations(seg1, seg2)
# Show the segmentations: plot coins, seg1, seg2 and segj side-by-side
```

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_515_0.png)

*图 10.16 – 集成分割*

如上所述，基于学习的经典分割使用手工制作的特征和机器学习模型进行逐像素分类，而集成方法则组合多个分割以获得稳健和准确的结果。

我们迄今为止学习了经典分割技术，包括阈值分割、watershed 和基于形态学的方法、超像素、变分模型、图形和概率方法，以及基于学习的经典分割。在这些基础之上，接下来我们将探索现代

用于语义和实例分割的深度学习方法，范围从基于 CNN 的架构如 FCNs 和 DeepLab 到基于 Transformer 的模型包括 DETR、SegFormer、Mask2Former 和 OneFormer。

## 深度学习用于图像分割

与之前探索的经典方法不同，经典方法中的分割依赖于手工制作的特征、阈值和启发式规则，深度学习直接从数据中学习分层表示。基于卷积和 Transformer 的架构建模复杂的空间模式，能够在光照、尺度、纹理和遮挡变化下实现鲁棒分割。

形式上，映射 $F_\theta$ 由具有参数 $\theta$ 的深度网络参数化，在数据集 $\{(I_i, Y_i)\}_{i=1}^N$ 上优化，通过最小化损失函数：

$\mathcal{L}(\theta) = \sum_{i=1}^N \ell\left(F_\theta(I_i), Y_i\right)$

其中 $\ell$ 衡量预测分割掩码与真实分割掩码之间的差异（例如，交叉熵、Dice 损失）。

与经典流程的关键区别在于，特征提取和决策制定在单个端到端可训练模型中统一。这使得网络能够自动学习难以手动设计的跨尺度、上下文感知的特征。

本部分考虑的三种范式在预测精细程度上有区别：

- - 语义分割为每个像素分配类别标签，而不区分对象实例。
- - 实例分割区分同一类别的各个对象实例。
- - 全景分割将两者统一，提供完整的场景理解，通过标记“事物”（可计数对象）和“东西”（无定形区域）。

本章的这一部分将发展这些范式的理论基础、架构原则和实际实现，突出显示深度模型如何重新定义图像分割的最新技术。对于基于深度学习的分割，建议从现在开始在 Google Colab 上使用 GPU 加速运行所有代码，以确保更快的推理和训练性能。

## 使用 CNN 的语义分割

在本节中，我们专注于使用预训练卷积神经网络（CNNs）进行语义分割，涵盖多个库和框架，如 PyTorch（torchvision、torch hub）、TensorFlow（Model Garden）、KerasHub、KerasCV 和 segmentation_models_pytorch。

为什么使用预训练模型？预训练模型是在大规模数据集如 ImageNet、Pascal VOC 和 COCO 上训练的网络，提供学习到的表示：

$f_\theta^{\text{pretrained}}: I \to Y.$

注意

虽然本章侧重于使用预训练模型进行推理，但下一章将涵盖从头训练分割模型、在自定义数据集上微调，以及领域适应和数据集特定优化。

### 全卷积网络

全卷积网络（FCNs）代表了语义分割的基础架构，通过消除全连接层并将其替换为卷积操作。设编码器网络提取分层特征：

$$F = \phi(I) \in \mathbb{R}^{h \times w \times C}$$

而不是展平 $F$，FCNs 应用 $1 \times 1$ 卷积：

$$Z = W * F \in \mathbb{R}^{K \times h \times w}$$

这会产生类别分数图，然后进行上采样：

$$\hat{Y} = \text{Upsample}(Z)$$

像素 $p$ 处的最终预测为：

$$\hat{y}_p = \operatorname{argmax}_k \operatorname{softmax}(Z)_k(p)$$

**跳跃连接：** 为了恢复空间细节，FCNs 组合不同深度的特征：

$$F_{\text{fused}} = F_{\text{deep}} + \sum_{l} \alpha_l F_l$$

其中 $F_l$ 是中间特征图。这里，深层 → 语义理解，浅层 → 空间精度，而融合 → 精确的密集预测。

**局限性：** 由于重复下采样和有限的多尺度上下文建模，导致边界模糊。

**使用 torchvision.models.segmentation 的 Python 演示**

集成分割工作流程通过以下阶段进行：

- - **模型加载：** 预训练的 FCN-ResNet50、DeepLabV3-ResNet50、DeepLabV3-MobileNetV3-Large 和 LR-ASPP-MobileNetV3-Large 模型从 torchvision.models.segmentation 加载，使用 .to(DEVICE) 移动到所选设备，并使用 .eval() 切换到评估模式。

