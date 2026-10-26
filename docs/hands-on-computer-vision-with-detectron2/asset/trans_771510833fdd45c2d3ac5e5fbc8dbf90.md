

**重要提示**

本章会在 Google Colab 实例上执行代码。你应该将 Google Drive 文件夹映射到实例中并把输出存储到映射的文件夹中，或者下载输出并保存以备将来使用。或者，如果你有本地实例，可以将这些 Google Colab 笔记本连接到本地实例，以便将结果保存在永久存储中并利用更好的计算资源。

## 数据处理

以下各节描述本章使用的数据集，并讨论在自定义数据集上训练 Detectron2 模型的典型步骤。这些步骤包括探索数据集、将数据集转换为 COCO 格式、向 Detectron2 注册数据集，最后展示一些示例图像及对应的脑肿瘤标签。

### 数据集

使用的数据集是 Kaggle（https://www.kaggle.com/datasets/davidroberts/brain-tumor-object-detection-datasets）上提供的脑肿瘤目标检测数据集，该数据集已下载到本书的 GitHub 仓库中以确保其可访问性。选择该数据集是因为医学图像处理是计算机视觉中的一个重要子领域。同时，该任务具有挑战性，且图像数量适合用于演示。

### 下载并执行初步探索

数据处理的第一步是下载并执行初步数据探索。以下代码片段下载该数据集并将其解压到当前工作目录中：

```
!wget https://github.com/PacktPublishing/Hands-On-Computer-Vision-with-Detectron2/blob/main/datasets/braintumors.zip?raw=true -O braintumors.zip
!unzip braintumors.zip -d braintumors
```

以下代码片段随后向我们展示该数据集中的文件夹：

```
import os
data_folder = "braintumors"
data_folder_yolo = data_folder + "_yolo"
data_folder_coco = data_folder + "_coco"
folders = os.listdir("braintumors")
print(folders)
```

具体来说，该数据集包含三个子文件夹：['coronal_t1wce_2_class', 'sagittal_t1wce_2_class', 'axial_t1wce_2_class']。下一步合理操作是使用 tree 命令打印数据集结构。因此，以下代码片段安装 tree 并显示该数据集的文件夹结构：

```
!sudo apt-get install tree
!tree braintumors/ -d
```

请注意，-d 选项仅显示目录，以避免因这些文件夹中文件数量众多而导致显示过长。具体来说，上述代码片段应显示以下文件夹结构：

```
braintumors/
├── axial_t1wce_2_class
│   ├── images
│   │   ├── test
│   │   └── train
│   └── labels
│       ├── test
│       └── train
├── coronal_t1wce_2_class
│   ├── images
│   │   ├── test
│   │   └── train
│   └── labels
│       ├── test
│       └── train
└── sagittal_t1wce_2_class
    ├── images
    │   ├── test
    │   └── train
    └── labels
        ├── test
        └── train
```

可以观察到，三个文件夹中的每一个都包含两个子文件夹，分别用于存放图像和标签。这些子文件夹中的每一个又有两个文件夹，分别用于训练数据和测试数据。需要注意的是，该数据集采用 YOLO 标注格式。

以下代码片段准备了一个辅助函数，用于统计采用 YOLO 标注格式的数据集中的图像和标签数量：

```
from glob import glob
def count_yolo_data(folder):
    for images_labels in ["images", "labels"]:
        for train_test in ["train", "test"]:
            file_ext = "*.jpg" if images_labels == "images" else "*.txt"
            p = os.path.join(folder, images_labels, train_test, file_ext)
            files = glob(p)
            print(train_test, images_labels, len(files))
```

然后，以下代码片段统计下载的脑肿瘤数据集中三个文件夹各自包含的图像和标签数量：

```
for folder in folders:
    print(" -"*8 + folder + "-"*8)
    folder = os.path.join(data_folder, folder)
    count_yolo_data(folder)
```

该代码片段应显示以下输出：

```
--------coronal_t1wce_2_class--------
train images 319
test images 78
train labels 318
test labels 78
--------sagittal_t1wce_2_class--------
train images 264
test images 70
train labels 264
test labels 70
--------axial_t1wce_2_class--------
train images 310
test images 75
train labels 296
test labels 75
```

值得注意的是，该数据集的图像数量相对较少，且有一些图像没有标签（图片中不存在脑肿瘤）。

一种方法是为每种图像类型（冠状面、矢状面或轴状面）分别训练一个模型。然而，由于图像数量较少，将这些文件夹中的图像合并到一个文件夹中可能更为合理。因此，以下代码片段准备了一个辅助方法，用于将 YOLO 文件从一个文件夹复制到另一个文件夹：

```python
import os
import shutil
from tqdm import tqdm

# move all into one folder
def copy_yolo_files(from_folder, to_folder, images_labels, train_test):
    from_path = os.path.join(from_folder, images_labels, train_test)
    to_path = os.path.join(to_folder, images_labels, train_test)
    os.makedirs(to_path, exist_ok=True)
    # get files
    file_ext = "*.jpg" if images_labels == "images" else "*.txt"
    files = glob(os.path.join(from_path, file_ext))
    # move files
    for file in tqdm(files):
        shutil.copy(file, to_path)
```

然后，以下代码片段将来自三个文件夹的图像合并到一个文件夹中：

```python
# copy
for from_folder in folders:
    from_folder = os.path.join(data_folder, from_folder)
    to_folder = data_folder_yolo
    for images_labels in ["images", "labels"]:
        for train_test in ["train", "test"]:
            copy_yolo_files(from_folder, to_folder, images_labels, train_test)
```

以下代码片段显示新合并数据集的结构：

```bash
# Now the folder should look like
!tree -d {data_folder_yolo}
```

现在，我们准备好使用第 3 章中介绍的代码片段将这些数据集转换为 COCO 格式。

### 数据格式转换

Detectron2 支持 COCO 数据格式，而当前数据集为 YOLO 格式。因此，以下代码片段执行数据格式转换。首先，运行以下代码片段安装 pylabel 包：

```
!pip install -q pylabel
```

接下来，以下片段创建一个辅助方法来执行数据转换（如第 3 章所述）：

```
from pylabel import importer

def yolo_to_coco(input_folder, output_folder, train_test):
    # Some codes are removed here (please refer to GitHub)
    # copy annotations
    for f in tqdm(txt_files):
        shutil.copy(f, coco_dir)
    # copy images
    for f in tqdm(img_files):
        shutil.copy(f, coco_dir)
    # get the classes
    with open(os.path.join(input_folder, "classes.txt"), "r") as f:
        classes = f.read().split("\n")
    # import
    dataset = importer.ImportYoloV5(path=coco_dir, cat_names=classes, name="brain tumors")
    # export
    coco_file = os.path.join(coco_dir, "_annotations.coco.json")
    # Detectron requires starting index from 1
    dataset.export.ExportToCoco(coco_file, cat_id_index=1)
    # now delete yolo annotations in coco set
    for f in txt_files:
        os.remove(f.replace(labels_path, coco_dir))
```

此时，如果所有这些数据处理步骤都是在 Google Colab 上执行的，那么最好将生成的 COCO 数据集下载到本地计算机，或者映射 Google Drive 文件夹并将生成的数据集保存到 Google Drive 文件夹中，以免在 Google Colab 实例关闭后丢失数据集。在本例中，转换后的 COCO 数据集已存储在本书的 GitHub 文件夹中，供后续章节使用。

现在数据集已准备好并正确注册，是时候训练模型并查看它们在使用 Detectron2 时的表现了。在自定义数据集上执行模型训练的最简单方法是使用 Detectron2 提供的默认训练器类。

### 使用默认训练器

Detectron2 提供了一个默认训练器类，可方便地在自定义数据集上训练 Detectron2 模型。首先，我们下载上一节中转换的数据集并解压：

```
!wget -q https://github.com/PacktPublishing/Hands-On-Computer-Vision-with-Detectron2/raw/main/datasets/braintumors_coco.zip
!unzip -q braintumors_coco.zip
```

接下来，安装 Detectron2，并使用上一节中给出的确切代码片段注册训练/测试数据集。此外，在训练之前，运行以下代码片段来准备一个记录器，Detectron2 将使用它来记录训练/推理信息：

```
from detectron2.utils.logger import setup_logger
logger = setup_logger()
```

注册好数据集并设置好记录器之后，下一步是获取训练配置。具体来说，我们设置输出目录（用于存储日志事件和训练后的模型）、用于保存配置文件的路径（该文件将用于未来的推理）、类别数为正负两类（nc = 2），以及将设备选项设置为 cuda（用于在 GPU 上训练）。最后，为了加快训练速度，我们还选择 ResNet50 FPN 作为要训练模型的骨干网络：

```
import os
from detectron2.config import get_cfg
from detectron2 import model_zoo
import pickle

output_dir = "output/object_detector"
os.makedirs(output_dir, exist_ok=True)
output_cfg_path = os.path.join(output_dir, "cfg.pickle")
nc = 2
```

