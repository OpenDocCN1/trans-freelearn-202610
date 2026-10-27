
+   - **图像预处理：** 使用OpenCV读取输入图像，通过`cv2.cvtColor()`从BGR转换为RGB，并通过`preprocess()`函数转换为网络所需的张量格式。
+   - **推理：** 在`torch.no_grad()`块内执行预测，以减少内存使用并提高执行效率。每个模型通过`model(input_tensor)["out"]`生成分割logits。
+   - **分辨率对齐：** 必要时，使用`F.interpolate()`将logits调整到公共空间分辨率，以便不同模型的预测能够一致地组合。
+   - **概率估计：** 使用`F.softmax(..., dim=1)`将对齐后的logits转换为类别概率图。然后对每个模型使用`torch.argmax()`获得逐像素分割掩码，并将结果掩码存储起来供后续可视化使用。
+   - **集成投票：** 将所有模型的概率图收集到`prob_maps`中，使用`torch.stack()`组合，并通过`torch.mean()`求平均，以执行软投票集成融合。通过对平均概率图应用`torch.argmax()`得到最终的集成分割掩码。
+   - **可视化：** 并排显示各个模型的掩码和最终的集成掩码，以便对各组成模型的预测与共识集成结果进行视觉比较。

现在参考下一个代码片段：

```
import torch, torch.nn.functional as F

from torchvision.models.segmentation import (
    fcn_resnet50, FCN_ResNet50_Weights,
    deeplabv3_resnet50, DeepLabV3_ResNet50_Weights,
    deeplabv3_mobilenet_v3_large, DeepLabV3_MobileNet_V3_Large_Weights,
    lraspp_mobilenet_v3_large, LRASPP_MobileNet_V3_Large_Weights)

models_dict = {"FCN-R50": \
    fcn_resnet50(weights=FCN_ResNet50_Weights.COCO_WITH_VOC_LABELS_V1),
    "DeepLab-R50": deeplabv3_resnet50( \
        weights=DeepLabV3_ResNet50_Weights.COCO_WITH_VOC_LABELS_V1),
    "DeepLab-Lite": deeplabv3_mobilenet_v3_large( \
        weights=DeepLabV3_MobileNet_V3_Large_Weights.COCO_WITH_VOC_LABELS_V1),
    "LRASPP": lraspp_mobilenet_v3_large( \
        weights=LRASPP_MobileNet_V3_Large_Weights.COCO_WITH_VOC_LABELS_V1)}

for name in models_dict:
    models_dict[name] = models_dict[name].to(DEVICE).eval()

img_rgb = cv2.cvtColor(cv2.imread("images/cycle.jpg"), cv2.COLOR_BGR2RGB)
```

```
```python
input_tensor = preprocess(img_rgb)  # refer to GitHub for preprocess()
prob_maps,individual_masks = [], []
with torch.no_grad():
    for name, model in models_dict.items():
        probs = F.softmax(model(input_tensor)["out"], dim=1)
        prob_maps.append(probs)
        individual_masks.append(
            torch.argmax(probs,dim=1).squeeze().cpu().numpy())
ensemble_prob = torch.mean(torch.stack(prob_maps), dim=0)  # Soft Voting
ensemble_mask = torch.argmax(ensemble_prob, dim=1).squeeze().cpu().numpy()
# Plot individual and ensemble masks here side-by-side
```
```

如果你运行上述代码，你应该得到如下图像：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_519_0.png)

*图10.17 – 使用预训练深度学习模型进行集成投票的图像分割*

尽管FCN开创了语义分割，但它们常常在边界精度和多尺度上下文建模方面遇到困难。*DeepLab*系列通过空洞卷积和*空洞空间金字塔池化*（ASPP）解决了这些挑战，在不牺牲空间分辨率的情况下实现更丰富的上下文理解。

## 空洞卷积与DeepLab系列

**DeepLab** 使用空洞（扩张）卷积改进FCN：

$$y[i] = \sum_k x[i + r \cdot k], w[k],$$

其中$r$是扩张率，能够在不降低空间分辨率的情况下获得更大的感受野。

利用空洞空间金字塔池化（ASPP）捕获多尺度上下文：

$$\mathrm{ASPP}(x) = \bigoplus_{r \in \{r_1,r_2,r_3,r_4\}} \mathrm{Conv}_r(x),$$

它在多个扩张尺度上聚合特征。DeepLab变体从DeepLabV1（CRF细化）、DeepLabV2（空洞卷积）、DeepLabV3（ASPP）逐步演进到DeepLabV3+（编码器-解码器细化）。

## 使用 `torchvision.models.segmentation` 的DeepLabV3

DeepLabV3推理流程与之前的FCN/Torchvision分割示例类似，留作读者练习（参考随附的GitHub仓库）。这里使用预训练的`deeplabv3_resnet101()`模型从输入图像生成语义分割图

使用Torchvision预处理、前向推理、通过F.interpolate()对logits进行上采样，以及使用torch.argmax()进行逐像素预测。下图展示了DeepLabV3的分割输出和叠加效果：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_520_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_520_1.png)

图10.18 - 使用torchvision.models.segmentation中的DeepLabV3进行图像分割

DeepLabV3不仅限于PyTorch生态系统。下面的示例使用来自TensorFlow Model Garden的预训练DeepLabV3模型演示相同的语义分割任务，使读者能够比较两个主流深度学习框架中的工作流程。

## 使用TensorFlow Model Garden的DeepLabV3

该代码演示了使用来自TensorFlow Model Garden的预训练DeepLabV3模型进行语义分割：下载并加载在Cityscapes数据集上训练的冻结推理图，将其包装为可调用的TensorFlow推理函数，并通过调整大小和NumPy转换对输入图像进行预处理，以匹配模型期望的输入格式。然后执行前向推理以获得逐像素语义预测，将预测的分割图调整回原始图像分辨率，将类别索引映射到语义标签和RGB颜色以进行可视化，并可选地加载地面真值掩码，用于分割性能的定性或定量评估。现在，参考以下代码片段：

```
tar = tarfile.open("models/deeplab_model.tar.gz")
for m in tar.getmembers():
    graph_def = tf.compat.v1.GraphDef.FromString(
        tar.extractfile(m).read())
    tar.close()
model = tf.function(lambda x:
    tf.import_graph_def(graph_def, input_map={"ImageTensor:0": x},
                        return_elements=["SemanticPredictions:0"]))

def segment(img_file):
    img = Image.open(img_file).convert("RGB")
    h, w = img.size[1], img.size[0]
    inp = np.asarray(img.resize((2049,1025)))[None]
    seg = model(inp)[0].numpy()[0]    # inference
    seg = cv2.resize(seg,(w,h),interpolation=cv2.INTER_NEAREST)
    return img, seg
```

函数`segment()`执行预处理，运行DeepLab模型生成逐像素分割，加载地面真值，并返回图像、预测和GT。按照下一个代码片段所示运行推理，获得如下图像：

```
img, seg = segment("images/mit_driveseg_sample.png", \
"images/mit_driveseg_sample_gt.png")
# Plot img, seg, overlay and gt side-by-side here
```

如果你运行上述代码，并将原始图像、预测的分割掩码和叠加图像并排绘制，你应该得到如下图：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_521_0.png)

图10.19 – 使用来自tensorflow model garden的预训练DeepLabV3模型对drive-seg示例图像进行分割

如果你将原始图像、地面真值分割掩码和叠加图像并排绘制，你应该得到如下图（比较这些图）：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_521_1.png)

图10.20 – 原始drive-seg示例图像和地面真值掩码

接下来，让我们评估模型以量化其性能：下面的代码通过`evaluate_single()`函数将预测标签与地面真值掩码进行比较来评估语义分割预测，通过逻辑掩码和索引统一标签映射，使预测类别与注释方案匹配。它使用掩码求和计算有效（非void）像素上的像素精度，使用`sklearn.metrics.confusion_matrix()`构建混淆矩阵以捕获逐类别的预测统计，并利用矩阵的对角线和行/列和推导出每个类别的交并比（IoU）值。最后，使用`np.mean()`计算各类别的平均IoU，并使用`tabulate()`以格式化表格呈现详细的逐类别评估结果。

```
def evaluate_single(seg_map, ground_truth):
    seg_map[np.logical_or(seg_map==14,seg_map==15)] = 13
```
