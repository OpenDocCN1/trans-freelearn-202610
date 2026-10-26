
这一简单的修改是 Detectron2 优秀设计的一个范例。只需简单地修改配置文件的链接，就可以相对容易地从一种应用类型切换到另一种应用类型。

## 执行推理

推理代码片段与上一节中开发的目标检测应用相同。然而，目标实例分割的输出有一些差异。具体来说，除了 `pred_boxes`、`scores` 和 `pred_classes`（分别对应边界框、预测置信度分数和相应的预测类别标签）之外，还有一个张量（`pred_masks`）用于指示输入图片中的每个像素是否属于某个检测到的实例（`True`/`False`）。以下是这个张量的示例（为了节省空间，输出的一部分被截断了），共有四个检测到的实例：

```
pred_masks: tensor([
  [[False, False, False, ..., False, False, False],
   ...,
   [False, False, False, ..., False, False, False]],

  [[False, False, False, ..., False, False, False],
   ...,
   [False, False, False, ..., False, False, False]],

  [[False, False, False, ..., False, False, False],
   ...,
   [False, False, False, ..., False, False, False]],

  [[False, False, False, ..., False, False, False],
   ...,
   [False, False, False, ..., False, False, False]],
  device='cuda:0')
```

## 可视化结果

得益于 Detectron2 的出色设计，可视化实例分割结果与可视化目标检测结果的方式相同。也就是说，代码片段保持一致。图 2.6 显示了带有实例分割结果注释的输入图像。具体来说，除了边界框和相应的预测置信度外，属于每个检测到的个体的像素也被高亮显示。

![](img/0e7994c850a84b7a53d34c7f943c7edd_48_0.png)

图 2.6：带有目标实例分割结果的推理输出

恭喜！你现在已经完成了一个使用 Detectron2 前沿模型的目标实例分割应用，与之前开发的目标检测应用相比，仅需少量修改。在下一节中，你将学习如何使用 Detectron2 开发关键点检测应用。

## 开发关键点检测应用

除了检测目标之外，关键点检测还会标出检测目标的重要部分，这些部分被称为**关键点**。这些关键点描述了被检测目标的基本特征。该特征通常对图像旋转、缩放、平移或变形保持不变。以下各节将详细介绍使用 Detectron2 预训练模型开发关键点检测应用的步骤。

```
[9, 9, 9, ..., 9, 9, 9],
[9, 9, 9, ..., 9, 9, 9],
[9, 9, 9, ..., 9, 9, 9]], device = 'cuda:0', dtype = torch.int32),
[{'id': 1, 'isthing': True, 'score': 0.9987247586250305, 'category_id': 0, 'instance_id': 0 },
{'id': 2, 'isthing': True, 'score': 0.997495174407959, 'category_id': 0, 'instance_id': 1 },
{'id': 3, 'isthing': True, 'score': 0.9887707233428955, 'category_id': 0, 'instance_id': 2 },
{'id': 4, 'isthing': True, 'score': 0.9777324199676514, 'category_id': 0, 'instance_id': 3 },
{'id': 5, 'isthing': False, 'category_id': 37, 'area': 15173 },
{'id': 6, 'isthing': False, 'category_id': 40, 'area': 163644 },
{'id': 7, 'isthing': False, 'category_id': 45, 'area': 192040 },
{'id': 8, 'isthing': False, 'category_id': 46, 'area': 62182 },
{'id': 9, 'isthing': False, 'category_id': 47, 'area': 164592 }])
```

具体来说，该字段是一个包含两个元素的元组。第一个元素是一个与输入图像尺寸相同的张量。该张量有助于为输入图像中的每个像素指示检测到的类别标签编号。该元组的第二个元素是一个项目列表，将相应的标签编号映射到已知类别。

## 可视化结果

全景分割的可视化代码片段与我们学过的其他视觉任务非常相似，不同之处在于我们需要从输出中提取 `panoptic_seg` 字段（前面已描述）的两个元素。接下来，这两个元素将被传递给 `Visualizer` 对象的 `draw_panoptic_seg_predictions` 方法以执行注释：

```
from google.colab.patches import cv2_imshow
from detectron2.utils.visualizer import Visualizer
from detectron2.data import MetadataCatalog
metadata = MetadataCatalog.get(cfg.DATASETS.TRAIN[0])
v = Visualizer(img[:, :, ::-1], metadata, scale=0.5)
```

## 选择配置文件和获取预测器

语义分割是全景分割的一个副产品。例如，如果检测到的目标位于同一区域，它会将属于相同类别的目标合并为一个，而不是为每个检测到的目标提供分割数据。因此，用于语义分割的模型与用于全景分割的模型相同。因此，配置文件、权重以及获取预测器的代码片段与之前全景分割应用中的相同。

## 执行推理

语义分割的推理代码片段也与全景分割相同。推理结果（与全景分割相同）也具有一个名为 `sem_seg` 的字段。该字段的形状为 类别数 × 图像高度 × 图像宽度。具体而言，在这种特定情况下为 54 × 720 × 960。尺寸 720 × 960 对应输入图像的所有像素。对于每个像素，有 54 个值，分别对应于该像素被分类为训练数据集中 54 个类别的概率。

## 可视化结果

语义分割结果的可视化略有不同。首先，提取 `sem_seg` 字段。接着，在 54 个概率中，使用 `argmax` 方法为每个像素选择预测概率最高的类别标签作为其标签。然后，将提取的数据传递给 `Visualizer` 对象的 `draw_sem_seg` 方法以进行可视化。以下是用于可视化语义分割结果的代码片段：

```
from google.colab.patches import cv2_imshow
from detectron2.utils.visualizer import Visualizer
from detectron2.data import MetadataCatalog
metadata = MetadataCatalog.get(cfg.DATASETS.TRAIN[0])
v = Visualizer(img[:, :, ::-1], metadata, scale=0.5)
sem_seg = output["sem_seg"].argmax(dim=0)
annotated_img = v.draw_sem_seg(sem_seg.to("cpu"))
cv2_imshow(annotated_img.get_image()[:, :, ::-1])
```

图 2.11 显示了在相应输入图像上可视化的语义分割结果。值得注意的是，与全景分割结果相比，检测到的人被分组为一个区域，并被归类为“物体”（things）。

```
from detectron2 import model_zoo
from detectron2.engine import DefaultPredictor
# We suppress some user warnings
import warnings
warnings.simplefilter(action='ignore',category=UserWarning)
# Create a configuration and a predictor
def get_predictor(config_file, checkpoint_url, score_thresh_test=1.0):
    # create a configuration object
    cfg = get_cfg()
    # get the configurations from the config_file
    config_file = model_zoo.get_config_file(config_file)
    cfg.merge_from_file(config_file)
    # get the pre-built weights of a trained model from the checkpoint
    cfg.MODEL.WEIGHTS = model_zoo.get_checkpoint_url(checkpoint_url)
    # set the threshold for recall vs. precision at test time
    cfg.MODEL.ROI_HEADS.SCORE_THRESH_TEST = score_thresh_test
    # create a predictor
    predictor = DefaultPredictor(cfg)
    return cfg, predictor
```

要获取模型，你只需选择配置文件和预训练权重的路径，并将它们传递给该方法。然后，该方法会返回一个预测器（`predictor`）以及该模型的配置（`cfg`）。获得预测器后，下一步是执行推理。

## 执行推理

所有计算机视觉任务的代码片段完全相同。因此，我们应该为此提供一个方法（`perform_inference`）。以下是给定任意预测器（`predictor`）和输入图像路径（`input_url`）时执行推理的代码片段：

```
import cv2
from google.colab.patches import cv2_imshow
def perform_inference(predictor, input_url):
    img = cv2.imread(input_url)
    output = predictor(img)
    return img, output
```

