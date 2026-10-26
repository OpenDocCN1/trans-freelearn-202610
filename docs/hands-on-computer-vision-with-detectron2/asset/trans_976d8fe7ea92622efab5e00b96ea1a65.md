

与任何预训练模型相关的下一个重要信息是预训练模型的架构。这通常通过模型在模型库（Model Zoo）中的名称来体现。例如，R50 和 R101 分别表示**微软亚洲研究院（MSRA）ResNet-50** 和 **ResNet-101**。类似地，X101 和 X152 分别表示 ResNeXt-101-32x8d 和 ResNeXt-152-32x8d。有些模型名称包含 FPN。这部分名称表示该模型使用了**特征金字塔网络（FPN）**技术。该技术意味着特征是在模型架构的不同层中提取的（而不是从最后一层获取输出特征并将其送入预测过程）。使用 FPN 主干网络有助于在时间权衡下提高准确率。此外，名称中包含 C4 表示该模型使用 conv4 主干网络。类似地，名称中包含 DC5 表示该模型使用在 conv5 中带有膨胀（dilation）的 conv5。

> **重要提示**
> 感兴趣的读者可以参阅 https://github.com/KaimingHe/deep-residual-networks 上关于 ResNet 架构的网页，以及 https://github.com/facebookresearch/ResNeXt 上关于 ResNeXt 架构的网页，以获取更多信息。

在了解了训练数据集和模型架构之后，下一步是检查训练和推理时间、训练内存以及模型准确率。这些信息有助于决定硬件需求以及速度与准确率之间的权衡。这些信息同样列在 Detectron2 模型库中的每个预训练模型下。

在根据速度与硬件需求之间的权衡决定要使用哪个模型之后，下一步是下载模型的配置文件。在模型库中，模型名称会链接到一个 .yaml 配置文件。**YAML**（**Yet Another Markup Language** 的缩写，或 **YAML ain't markup language**）是一种用于数据序列化的人类可读编程语言。它通常用于编写配置文件和支持自动化流程。最后，还有一个用于下载预训练模型权重的链接。

到目前为止，你应该已经能够阅读并理解 Detectron2 模型库中可用预训练模型的重要信息。下一步是使用一些预训练模型来执行重要的计算机视觉任务。使用 Detectron2 开发计算机视觉应用通常包括以下步骤：

- 选择配置文件
- 获取预测器
- 执行推理
- 可视化结果

以下各节将详细介绍开发目标检测应用的这些步骤。

## 选择模型
```
config_file = "COCO-Detection/faster_rcnn_X_101_32x8d_FPN_3x.yaml"
checkpoint_url = "COCO-Detection/faster_rcnn_X_101_32x8d_FPN_3x.yaml"
# Create a configuration file
cfg = get_cfg()
config_file = model_zoo.get_config_file(config_file)
cfg.merge_from_file(config_file)
# Download weights
cfg.MODEL.WEIGHTS = model_zoo.get_checkpoint_url(checkpoint_url)
score_thresh_test = 0.95
cfg.MODEL.ROI_HEADS.SCORE_THRESH_TEST = score_thresh_test
predictor = DefaultPredictor(cfg)
```

具体来说，get_cfg 方法有助于获取该模型的默认配置对象。类似地，model_zoo 有助于获取配置文件（.yaml 文件）并下载预训练权重。同时，需要 DefaultPredictor 来根据下载的配置文件和权重创建预测器。除了必需的导入之外，这段代码还抑制了几条可以安全忽略的用户警告。

值得注意的是，cfg.MODEL.ROI_HEADS.SCORE_THRESH_TEST 参数当前设置为 0.95。该参数在测试时用于过滤掉所有检测置信度低于此值的检测对象。将此参数设置为较高值（接近 1.0）意味着我们希望获得高精确率和低召回率。同样，将其设置为较低值（接近 0.0）意味着我们更倾向于低精确率和高召回率。一旦我们有了模型（或者可以互换地称之为预测器），就可以开始执行推理了。

## 执行推理

以下代码片段对输入图像执行目标检测。具体来说，我们首先导入 cv2 以从 input_url 读取图像。接下来，使用之前创建的预测器，即可相对轻松地对加载的图像执行目标检测（请将图像上传到 Google Colab 以执行推理）：

```
import cv2
input_url = "input.jpeg"
img = cv2.imread(input_url)
output = predictor(img)
```

下一个重要任务是理解 Detectron2 给出的输出格式以及每个输出组件的含义。此处的 print (output) 显示了输出变量及其提供的字段：

```
{'instances': Instances(num_instances = 4, image_height = 720, image_width = 960, fields = [pred_boxes: Boxes(tensor([[492.1558, 331.5353, 687.8397, 637.9542], [294.6652, 192.1288, 445.8517, 655.3390], [419.8898, 253.9013, 507.7427, 619.1491], [451.8833, 395.1392, 558.5139, 671.0604]], device = 'cuda:0')), scores: tensor([0.9991, 0.9986, 0.9945, 0.9889], device = 'cuda:0'), pred_classes: tensor([0, 0, 0, 0], device = 'cuda:0')]) }
```

可以观察到，除了图像信息之外，目标检测结果还包括检测到的实例（instances），以及相应的预测边界框（pred_boxes）、预测置信度分数（scores）和预测类别标签（pred_classes）。有了这些信息，我们就可以将检测到的对象及其对应的边界框可视化。

## 可视化结果

Detectron2 提供了一个 Visualizer 类，它可以帮助你轻松地将检测结果标注到输入图像上。因此，以下代码片段导入该类并使用其可视化方法将检测结果标注到输入图像上：

```
from google.colab.patches import cv2_imshow
from detectron2.utils.visualizer import Visualizer
from detectron2.data import MetadataCatalog
metadata = MetadataCatalog.get(cfg.DATASETS.TRAIN[0])
v = Visualizer(img[:, :, ::-1], metadata, scale=0.5)
instances = output["instances"].to("cpu")
annotated_img = v.draw_instance_predictions(instances)
cv2_imshow(annotated_img.get_image()[:, :, ::-1])
```

具体来说，第一条语句导入 cv2_imshow 方法（一个 Google Colab 补丁），用于在 Google Colab 输出中显示图像。下一条导入语句是 Visualizer 本身。第三条导入语句帮助获取有关训练数据集的信息。具体来说，这个 MetadataCatalog 对象为预测对象提供标签名称。在本例中，类别标签 0 对应的是 person（人），如输出中所示。

## 选择配置文件

与目标检测类似，Detectron2 也提供了一系列针对对象实例分割任务预训练的前沿模型。例如，图 2.5 展示了在 COCO 实例分割数据集上预训练的 Mask R-CNN 模型列表。

| 名称 | 学习率调度 | 训练时间（秒/迭代） | 推理时间（秒/图像） | 训练内存（GB） | 边界框 AP | 掩码 AP | 模型 ID | 下载 |
|---|---|---|---|---|---|---|---|---|
| R50-C4 | 1x | 0.584 | 0.110 | 5.2 | 36.8 | 32.2 | 137259246 | model | metrics |
| R50-DC5 | 1x | 0.471 | 0.076 | 6.5 | 38.3 | 34.2 | 137260150 | model | metrics |
| R50-FPN | 1x | 0.261 | 0.043 | 3.4 | 38.6 | 35.2 | 137260431 | model | metrics |
| R50-C4 | 3x | 0.575 | 0.111 | 5.2 | 39.8 | 34.4 | 137849525 | model | metrics |
| R50-DC5 | 3x | 0.470 | 0.076 | 6.5 | 40.0 | 35.9 | 137849551 | model | metrics |
| R50-FPN | 3x | 0.261 | 0.043 | 3.4 | 41.0 | 37.2 | 137849600 | model | metrics |
| R101-C4 | 3x | 0.652 | 0.145 | 6.3 | 42.6 | 36.7 | 138363239 | model | metrics |
| R101-DC5 | 3x | 0.545 | 0.092 | 7.6 | 41.9 | 37.3 | 138363294 | model | metrics |
| R101-FPN | 3x | 0.340 | 0.056 | 4.6 | 42.9 | 38.6 | 138205316 | model | metrics |
| X101-FPN | 3x | 0.690 | 0.103 | 7.2 | 44.3 | 39.5 | 139653917 | model | metrics |

图 2.5：基于 Mask R-CNN 的 COCO 实例分割基线

在检查了这些模型的规格后，本特定应用选择了 X101-FPN 模型。以下是该配置文件的相对路径（从模型名称链接的地址中提取并截取）：

```
COCO-InstanceSegmentation/mask_rcnn_X_101_32x8d_FPN_3x.yaml
```

## 获取预测器

用于获取此对象实例分割预测器的代码片段与之前开发的目标检测应用相同，除了以下两行。这两行指定了该特定预训练模型的不同配置文件：

```
# Select a model
config_file = "COCO-InstanceSegmentation/mask_rcnn_X_101_32x8d_FPN_3x.yaml"
checkpoint_url = "COCO-InstanceSegmentation/mask_rcnn_X_101_32x8d_FPN_3x.yaml"
```

