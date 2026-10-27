

## 多源卫星影像分割与混合注意力 UNet

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_589_0.png)

*Figure 11.20 – Segmenting satellite image patches with MA-UNet*

多类 Dice 分数针对每个类别进行计算。最终分数为：

$$\mathrm{Dice}_{\mathrm{mean}} = \frac{1}{C}\sum_{c=1}^{C} \mathrm{Dice}_c$$

Dice 分数接近 1 表示分割重叠度极佳。

预测的标签掩膜通过函数 `decode_mask()` 转换回 RGB 颜色。这会生成可解释的土地覆盖图，其中每个语义类别都使用不同的颜色显示。

典型的可视化包括原始卫星图像、真实掩膜和预测分割图并排显示。这种定性分析至关重要，因为分割质量通常取决于边界精度和区域一致性，而不仅仅是数值指标。

```python
i = 1
img_path, mask_path = imgs[i], masks[i]
img = cv2.imread(img_path)
mask = cv2.imread(mask_path)
img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)
mask = cv2.cvtColor(mask, cv2.COLOR_BGR2RGB)
H, W, _ = img.shape

# pad image to multiple of PATCH
pad_h = (PATCH - H % PATCH) % PATCH
pad_w = (PATCH - W % PATCH) % PATCH
img_pad = np.pad(img, ((0, pad_h), (0, pad_w), (0, 0)), mode='reflect')
mask_pad = np.pad(mask, ((0, pad_h), (0, pad_w), (0, 0)), mode='reflect')
Hp, Wp, _ = img_pad.shape
```

尽管采用了有效的分块、注意力融合和类别不平衡处理，遥感分割仍然具有挑战性，原因在于高分辨率复杂性、多传感器变异性和严重的类别不平衡，这促使了像混合注意力 U-Net 这样专门的架构，以实现更好的全局上下文建模和精细细节保留。

MAnet 架构提供了若干重要优势：

- 改进的上下文推理
- 增强的边界定位
- 更好的小目标检测
- 高效的编码器-解码器融合
- 对类别不平衡的鲁棒性

注意力机制在遥感中特别有用，因为卫星场景包含跨越多个尺度的高度异构空间结构。

## 应用

混合注意力卫星分割系统广泛应用于土地覆盖分类、洪水制图、作物监测、野火评估、城市扩张分析、军事侦察、海岸线提取、道路网络检测和环境监控。这些系统构成了现代地理空间 AI 管道的关键组成部分。

在探索了涵盖经典、深度学习和基于 Transformer 的范式的各种分割方法后，我们现在以介绍用于定量评估分割性能的标准评估指标作为本章的结尾。

## 分割评估指标

评估分割模型的质量与设计模型本身一样重要。在语义分割和实例分割中，预测掩膜 Ŷ 必须与真实掩膜 Y 进行比较，以量化模型识别对象边界和语义区域的准确性。

设 $Y \subseteq \Omega$ 表示属于真实对象区域的像素集合，$\hat{Y} \subseteq \Omega$ 表示预测的分割区域，其中 $\Omega$ 为图像域。

对于二值分割，每个像素要么属于前景类，要么属于背景类。真实类别与预测类别之间的比较通常使用混淆矩阵量来表达：

- **True Positive (TP):** 正确预测的前景像素
- **False Positive (FP):** 错误预测为前景的背景像素
- **False Negative (FN):** 模型遗漏的前景像素
- **True Negative (TN):** 正确预测的背景像素

这些量构成了大多数分割指标的基础。

## 交并比指标

交并比 Intersection over Union (IoU) 指标，也称为 Jaccard Index，衡量预测区域与真实区域之间的重叠程度：

IoU = |Y ∩ Ŷ| / |Y ∪ Ŷ|

其中 Y ∩ Ŷ 表示交集（共同像素），Y ∪ Ŷ 表示并集（属于任一掩膜的所有像素）。

使用混淆矩阵术语，

IoU = TP / (TP + FP + FN)

其中 IoU = 1 为完美分割，IoU = 0 表示无重叠。IoU 越高，表示定位和边界一致性越好。IoU 同时惩罚假阳性和假阴性，使其成为语义分割最可靠的指标之一。

IoU 广泛用于医学图像分割、自动驾驶、卫星图像分析、器官和肿瘤分割，以及 Cityscapes 和 Pascal VOC 等基准数据集。对于多类分割，IoU 对每个类别独立计算并取平均：

mIoU = 1/C Σ_{c=1}^C IoU_c

其中 C 为类别数。

## Dice 系数

Dice 相似系数 Dice Similarity Coefficient (DSC) 衡量两个集合之间的相似性，在医学成像中特别流行：

Dice = 2|Y ∩ Ŷ| / (|Y| + |Ŷ|)

用混淆矩阵量表示，

Dice = 2TP / (2TP + FP + FN)

Dice 系数与 IoU 密切相关：

Dice = 2·IoU / (1 + IoU)

反之，

IoU = Dice / (2 − Dice)

### Note

Dice 的取值范围为 0 到 1。Dice 越高，表示重叠越强。Dice 比 IoU 对小结构更敏感。这一特性使 Dice 特别适用于肿瘤分割、病变检测、血管分割和脑 MRI 分析。

## Dice 损失

深度学习模型通常优化 Dice 损失：

```
L_Dice = 1 − Dice
```

或其可微分的软版本：

```
L_Dice = 1 − (2 Σ_i p_i g_i + ε) / (Σ_i p_i + Σ_i g_i + ε)
```

其中 p_i 为预测概率，g_i 为真实标签，ε 用于防止除零。Dice 损失对于前景仅占图像一小部分的失衡数据集非常有效。

## 像素准确率

像素准确率衡量正确分类像素的比例：

```
Accuracy = correct pixels / total pixels
```

对于二值分割，

```
Accuracy = (TP + TN) / (TP + TN + FP + FN)
```

像素准确率评估全局分类正确性，但在高度不平衡的数据集上可能具有误导性。有时会出现准确率很高，但分割完全无用的情况，因为对象从未被检测到。因此，像素准确率不应单独用于分割评估。

## 平均像素准确率

对于多类分割，我们有：

```
Mean Accuracy = (1/C) Σ_{c=1}^C TP_c / (TP_c + FN_c)
```

其中 C 为语义类别数。

## 平均平均精度

平均平均精度 Mean Average Precision (mAP) 指标在实例分割和目标检测中广泛使用，特别是对于 Mask R-CNN、YOLO 和 Detectron2 等模型。与语义分割指标不同，mAP 同时评估：

- 定位质量
- 分类正确性

## 精确率和召回率

精确率定义为：

Precision = TP / (TP + FP)

而召回率为：

Recall = TP / (TP + FN)

如果其 IoU 超过阈值，则认为预测是正确的，通常为：IoU ≥ 0.5。该标准通常写为 AP₅₀。

## 平均精度

对于给定类别，在不同置信度阈值下计算精确率-召回率曲线。平均精度 AP 为该曲线下的面积：

AP = ∫₀¹ p(r) dr,

其中 p(r) 是召回率的函数。

平均平均精度：所有类别的平均值给出：

mAP = 1/C Σ_{c=1}^C AP_c

## COCO 风格的 mAP

COCO 基准在多个 IoU 阈值上评估 AP：{0.50,0.55,0.60,...,0.95} 并取平均：

mAP_coco = 1/10 Σ_{t=0.50}^{0.95} AP_t

这比仅使用 AP₅₀ 产生更严格的评估。

## 获取本书的 PDF 版本和更多内容

扫描二维码（或访问 packtpub.com/unlock）。按书名搜索本书，确认版本，然后按照页面上的步骤操作。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_596_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_596_1.png)

Note: Keep your invoice handy. Purchases made directly from Packt don’t require an invoice.

