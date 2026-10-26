
这里是信息部分：

```
"info": {
    "year": "2022",
    "version": "1",
    "description": "Brain tumor object detection",
    "contributor": "",
    "url": "<the URL to Kaggle dataset>",
    "date_created": "2022-10-07T13:54:09+00:00"
},
```

这是许可证部分：

```
"licenses": [
    {
        "id": 1,
        "url": "https://creativecommons.org/publicdomain/zero/1.0/",
        "name": "CC0 1.0"
    }
],
```

这是类别部分：

```
"categories": [
    {
        "id": 0,
        "name": "tumors",
        "supercategory": "none"
    },
    {
        "id": 1,
        "name": "negative",
        "supercategory": "tumors"
    },
    {
        "id": 2,
        "name": "positive",
        "supercategory": "tumors"
    }
],
```

这里是图像部分：

```
"images": [
    {
        "id": 0,
        "license": 1,
        "file_name": "00000_102.jpg",
        "height": 512,
        "width": 512,
        "date_captured": "2022-10-07T13:54:09+00:00"
    },
    {
        "id": 1,
        "license": 1,
        "file_name": "00003_154.jpg",
        "height": 512,
        "width": 512,
        "date_captured": "2022-10-07T13:54:09+00:00"
    }
],
```

这里是标注部分：

```
"annotations": [
    {
        "id": 0,
        "image_id": 0,
        "category_id": 2,
        "bbox": [170,226,49,51],
        "area": 2499,
        "segmentation": [],
        "iscrowd": 0
    },
    {
        "id": 1,
        "image_id": 1,
        "category_id": 1,
        "bbox": [197,143,80,62],
        "area": 4960,
        "segmentation": [],
        "iscrowd": 0
    }
]
```

值得注意的是，边界框以绝对像素位置指定。坐标系以高度为 y 轴，宽度为 x 轴，原点位于图片左上角。例如，对于第一张图片（00003_154.jpg 且 image_id = 1），边界框为 [197,143,80,62]，分别对应 [x_min, y_min, width, height] 的值，如图 3.5 所示。

![](img/0e7994c850a84b7a53d34c7f943c7edd_71_0.png)

图 3.5：COCO 风格边界框的坐标系和标注格式（[x_min, y_min, width, height]）

另一种流行的图像标注格式是 Pascal VOC（Pattern Analysis, Statistical Modeling, and Computational Learning, Visual Object Classes）格式。这种标注格式采用 XML（eXtensible Markup Language）文件格式，每张图像对应一个 XML 文件。

标注文件名与图像文件名相同，只是扩展名改为 .xml。每张图像单独存储一个 XML 文件，可以避免为大型图像数据集的所有标注维持一个巨大文件。以下是演示数据集中一张图像（00003_154.jpg）的标注文件（00003_154.xml）：

```
<annotation>
    <folder></folder>
    <filename>00003_154.jpg</filename>
    <path>00003_154.jpg</path>
    <source>
        <database>annotationdemo</database>
    </source>
    <size>
        <width>512</width>
        <height>512</height>
        <depth>3</depth>
    </size>
    <segmented>0</segmented>
    <object>
        <name>negative</name>
        <pose>Unspecified</pose>
        <truncated>0</truncated>
        <difficult>0</difficult>
        <occluded>0</occluded>
        <bndbox>
            <xmin>197</xmin>
            <xmax>277</xmax>
            <ymin>143</ymin>
            <ymax>205</ymax>
        </bndbox>
    </object>
</annotation>
```

显然，该标注文件包含图像信息和标注对象。如图 3.6 所示，图像坐标系与之前描述的 COCO 标注格式相同。它同样有四个边界框数值：xmin、xmax、ymin 和 ymax。换句话说，边界框使用左上角和右下角坐标的绝对像素值存储（而不是左上角坐标加宽高值）。

![](img/0e7994c850a84b7a53d34c7f943c7edd_73_0.png)

图 3.6：Pascal VOC 风格边界框的坐标系和标注格式（[xmin, xmax, ymin, ymax]）

由于 YOLO（You Only Look Once）近年来的发展，YOLO 数据标注成为另一种流行的数据格式。与 PASCAL VOC 标注风格类似，该数据集每张图像对应一个标注文件。不过，标注文件的扩展名是 .txt。

数据集组织为两个子文件夹，一个用于图像（images），一个用于标注（labels）。这些子文件夹可能进一步划分为 train（训练）、test（测试）和 val（验证）子文件夹。还有一个名为 classes.txt 的文本文件（也可能是 *.yaml 文件），用于声明该数据集的类别或标签，每行一个。以下是我们简单演示数据集的 classes.txt 文件内容，包含两个类别：

```
negative
positive
```

类似地，使用 YOLO 格式标注图像的步骤如下：

1. 创建一个名为 `classes.txt` 的文件，其中包含标签列表（每行一个标签）。
2. 按照之前的说明启动 `labelImg`。
3. 点击 **Open Dir** 菜单打开图像目录（如果在启动时没有指定图像目录）。
4. 如果当前格式不是 YOLO（当前可能是 CreateML 或 Pascal VOC），点击 **CreateML** 或 **Pascal VOC** 菜单，直到其切换为 **YOLO**。
5. 点击 **Change Save Dir** 菜单更改标注目录。

每张图像对应一个带有 YOLO 格式标注的 `.txt` 文件，保存到所选目录中，文件名与图像同名。名为 `classes.txt` 的文件也会保存在该文件夹中。

此外，以下快捷键有助于快速标注：

- *W 键*：创建边界框
- *D 键*：切换到下一张图像
- *A 键*：切换到上一张图像
- *Delete 键*：删除选中的边界框
- *方向键*：移动选中的边界框
- *Ctrl + Shift + D*：删除当前图像

从不同来源或数据标注工具获取的数据集，其标注格式可能与 Detectron2 支持的格式（COCO 数据格式）不同。因此，了解如何将不同格式的数据集转换为 COCO 格式至关重要。下一节将引导你完成这一数据集转换过程。

## 标注格式转换

Detectron2 的数据描述基于 COCO 标注格式构建。换句话说，它支持使用 COCO 数据标注格式注册数据集。然而，其他数据标注格式大量存在，你可能下载的数据集或使用的标注工具支持与 COCO 不同的数据格式。因此，本节将介绍用于将流行的 Pascal VOC 和 YOLO 格式转换为 COCO 风格的代码片段。

> **重要提示**
> 以感叹号（!）开头的语句表示需要在 Jupyter notebook（Google Colab）代码单元中执行的 Bash 命令。如果你想在终端中运行，可以安全地移除这个感叹号并执行该语句。

### 将 Pascal VOC 数据集转换为 COCO 数据集

Pascal VOC 标注格式是另一种流行的格式。因此，本节提供代码片段，用于在注册到 Detectron2 之前将数据转换为 COCO 标注格式。本节使用前面描述的简单数据集（annotationdemo）作为演示。首先，我们下载 Pascal VOC 格式的脑肿瘤数据集（voc.zip）并在 Google Colab 上解压：

```
# 下载数据集并解压
!wget url_to_voc.zip
!unzip voc.zip
```

数据集解压后，以下代码片段安装 tree 包并查看该数据集的结构：

```
!sudo apt-get install tree
!tree voc
```

以下是该数据集在 COCO 标注格式下的结构：

```
voc
└ test
    ├── 00000_102.jpg
    ├── 00000_102.xml
    ├── 00003_154.jpg
    └── 00003_154.xml
```

值得注意的是，Pascal VOC 数据格式为每个图像文件存放一个同名、扩展名为 .xml 的标注文件。

该数据集可以直接原地修改为 COCO 格式。不过，以下代码片段会将这些图像和标注复制到一个新目录（coco_dir）中用于 COCO 数据集，从而避免修改原始数据：

```
import os
import shutil
from glob import glob
from tqdm import tqdm

voc_dir = "voc/test"
coco_dir = 'coco/test'
os.makedirs(coco_dir, exist_ok=True)
```

