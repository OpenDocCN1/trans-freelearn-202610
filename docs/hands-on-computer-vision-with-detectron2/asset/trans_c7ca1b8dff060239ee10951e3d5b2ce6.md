

## PointRend 简介

PointRend 是 Detectron2 的一个项目。它有助于为检测到的物体边界提供更好的分割质量。它可以与实例分割和语义分割一起使用。它是 Mask R-CNN 头部的扩展，我们在上一章中讨论过它。它对检测到的掩码进行点采样，并在采样的点上进行预测，而不是对掩码中的所有点进行预测。这种技术使我们能够以更高的分辨率计算掩码，从而提供更高的掩码分辨率。图 11.1 举例说明了在使用（左）和使用（右）PointRend 时的两个图像：

![](img/0e7994c850a84b7a53d34c7f943c7edd_251_0.png)

![](img/0e7994c850a84b7a53d34c7f943c7edd_251_1.png)

图 11.1：使用和不使用 PointRend 的分割质量

PointRend 有助于渲染更高分辨率的分割，其物体边界清晰且不那么平滑。因此，如果要检测的物体具有尖锐边缘，则很有用。

在推理时，从粗略预测开始，它通过多个步骤进行上采样以提高掩码分辨率。在每个步骤中，有三个主要任务：

- 1. 采样一定数量（超参数）的点以进行预测。这种策略有助于避免在更高分辨率下对许多网格点产生计算预测。
- 2. 从主干提取的特征图以及来自粗略预测的特征中为每个采样点提取特征。
- 3. 使用多层感知器神经网络（Point Head）使用其提取的特征对每个采样点的标签进行分类。

图 11.2 说明了推理时 PointRend 策略在三个不同分辨率下对一个物体实例的处理：

![](img/0e7994c850a84b7a53d34c7f943c7edd_252_0.png)

图 11.2：推理时 PointRend 预测

最左边的图像是上一章讨论的 7×7 粗略预测。PointRend 对要采样的点数有一个超参数（N = 14×14，在本例中）；因此，粗略预测被上采样（例如，使用双线性插值），如前一图中间所示。此解决方案具有与要采样的点数相同数量的像素。因此，每个点都传递给 Point Head 以预测其标签（图中间）。在下一个细分步骤中，图像被上采样到 28×28 分辨率。此分辨率的像素数多于 N 个点。因此，PointRend 在此处应用采样策略。N 个采样的点是更模糊的点（就它们是否应属于物体而言，它们代表边界）。换句话说，这些采样为 14×14 的点传递给 Point Head 以进行预测，其预测概率接近 0.5（模糊）。原因很明显：对背景或物体内的置信度很高的点不需要进行进一步预测。这种方法在推理时是迭代的。

这种迭代方法不利于训练神经网络。因此，PointRend 在训练时使用不同的点采样策略来训练其 Point Head。该采样策略有三个参数：要采样的点数（N）、上采样常数（k）和重要性采样比率（β），即更接近边缘（模糊或预测置信度接近 0.5）的点的百分比。具体来说，这些点是通过以下步骤选择的：

- 1. 在粗略预测中均匀采样 kN 个点（采样 kN 而不是 N 使此步骤成为过采样步骤，k 为过采样常数，其中 k > 1）。
- 2. 选择 βN 个最接近边界的点（按其预测置信度与 0.5 的接近程度排名）。这些点更接近边缘，这使它们很重要。因此，β 被称为重要性采样比率。
- 3. 从步骤 1 中采样的 kN 个点中选择 (1-β)N 个点，以便为训练 Point Head 共有 N 个点。

图 11.3 说明了所选点（右）的示例，用于 7×7 分辨率的粗略掩码预测（左），其中 N = 14×14，k = 3 且 β = 0.75：

![](img/0e7994c850a84b7a53d34c7f943c7edd_253_0.png)

图 11.3：训练时 PointRend 点采样策略

选择了更多点（大约 75% 在边界处），其余点在粗略掩码上均匀随机选择以提取特征并训练 Point Head。

像其他 Detectron2 模型一样，PointRend 项目为其 Model Zoo 提供了一组内置模型，可以通过 Detectron2 GitHub 仓库中的 projects/PointRend 文件夹访问。我们在第 2 章中使用的相同分析可用于选择合适的 PointRend 模型，在准确性和推理时间之间进行权衡。下一节演示了使用 PointRend Model Zoo 中现有模型创建物体实例分割应用程序的步骤，并说明了这些步骤的工作原理。

## 使用现有 PointRend 模型

使用现有 PointRend 模型进行物体实例分割的步骤与使用上一章中描述的 Detectron2 Model Zoo 中现有 Detectron2 模型进行物体实例分割的步骤相似。因此，本节涵盖更多差异。对于 PointRend，我们需要克隆 Detectron2 仓库以使用 PointRend 项目的配置文件：

```
!git clone --branch https://github.com/facebookresearch/detectron2.git detectron2_repo
```

仓库存储在当前工作目录中的 detectron2_repo 文件夹中。克隆此仓库后，生成配置的代码略有不同：

```
# some other common import statements are removed here
from detectron2.projects import point_rend
config_file = "detectron2_repo/projects/PointRend/configs/
InstanceSegmentation/pointrend_rcnn_X_101_32x8d_FPN_3x_coco.
yaml"
checkpoint_url = "detectron2://PointRend/InstanceSegmentation/
pointrend_rcnn_X_101_32x8d_FPN_3x_coco/28119989/model_final_
ba17b9.pkl"

cfg = get_cfg()
point_rend.add_pointrend_config(cfg)
cfg.merge_from_file(config_file)
cfg.MODEL.WEIGHTS = checkpoint_url
score_thresh_test = 0.95
cfg.MODEL.ROI_HEADS.SCORE_THRESH_TEST = score_thresh_test
predictor = DefaultPredictor(cfg)
```

具体来说，此代码片段利用从 PointRend Model Zoo 中选择的现有配置文件和模型权重。它还使用从 Detectron2 仓库克隆的文件添加 PointRend 特定配置。生成配置文件、下载现有权重并使用 DefaultPredictor 创建预测器的其余代码类似。

以下代码片段创建了一个辅助方法来以原始大小可视化给定的图像：

```
def imshow(image):
    dpi = plt.rcParams["figure.dpi"]
    im_data = image[:, :, :-1]
    height, width, depth = im_data.shape
    figsize = width / float(dpi), height / float(dpi)
    fig = plt.figure(figsize=figsize)
    plt.imshow(im_data)
    plt.imshow(im_data)
    plt.axis("off")
    plt.show()
```

在 PointRend Model Zoo 上使用现有模型可以满足使用计算机视觉任务流行数据集训练的模型的最常见需求。然而，如果需要，PointRend 也允许我们在自定义数据集上训练其模型。下一节描述了在自定义数据集上训练 PointRend 模型的步骤。

## 训练自定义 PointRend 模型

本节描述了在脑肿瘤数据集（上一章中描述）上为物体实例分割任务训练自定义 PointRend 模型的步骤。训练自定义 PointRend 模型包括与在 Detectron2 中训练实例分割模型相似的步骤。因此，本节更侧重于两者之间的差异；完整源代码可在本书的 GitHub 仓库中获得。

下载脑肿瘤分割数据集、安装 Detectron2 并注册训练和测试数据集的源代码保持不变。与上一节类似，在我们获取配置文件之前，我们需要克隆 Detectron2 仓库以使用 PointRend 项目的配置文件：

```
!git clone https://github.com/facebookresearch/detectron2.git
detectron2_repo
```

初始配置的代码保持不变：

```
output_dir = "output/pointrend"
os.makedirs(output_dir, exist_ok=True)
output_cfg_path = os.path.join(output_dir, "cfg.pickle")
nc = 3
device = "cuda" if torch.cuda.is_available() else "cpu"
```

此外，以下代码片段与上一节中从 PointRend Model Zoo 中选择现有 PointRend 模型时显示的代码片段类似：

```
from detectron2.projects import point_rend
config_file = "detectron2_repo/projects/PointRend/configs/
InstanceSegmentation/pointrend_rcnn_R_50_FPN_3x_coco.yaml"
checkpoint_file = "detectron2://PointRend/InstanceSegmentation/
pointrend_rcnn_R_50_FPN_3x_coco/164955410/model_final_edd263.pkl"
cfg = get_cfg()
point_rend.add_pointrend_config(cfg)
cfg.merge_from_file(config_file)
cfg.MODEL.WEIGHTS = checkpoint_file
```

