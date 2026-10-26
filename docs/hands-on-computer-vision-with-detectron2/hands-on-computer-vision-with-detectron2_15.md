## 13
将 Detectron2 模型部署到浏览器和移动环境

本章介绍开放神经网络交换（ONNX）框架。该框架有助于在多个框架和平台之间共享深度神经网络。当需要将 Detectron2 模型部署到浏览器或移动环境时，它非常有帮助。本章还描述了 D2Go，这是一个用于以最小的内存存储和计算需求训练、量化并部署模型的框架。使用该框架创建的模型对于部署到移动或边缘设备非常有帮助。

到本章结束时，你将了解 ONNX 框架及其适用场景。你可以将自定义 Detectron2 模型导出为此格式并部署到 Web 或浏览器环境。你还将了解 D2Go 框架的优势，并知道如何利用其 Model Zoo 上的预训练模型或使用 D2Go 训练自定义模型。你还可以对自定义模型进行量化，以进一步优化其内存并提高其计算时间。

具体来说，本章涵盖以下主题：

- 使用 ONNX 部署 Detectron2 模型
- 使用 D2Go 开发移动计算机视觉应用

## 技术要求

你应已完成第 1 章，以便为 Detectron2 拥有合适的开发环境。
所有代码、数据集和结果均可在本书的 GitHub 页面上获取，地址为 https://github.com/PacktPublishing/Hands-On-Computer-Vision-with-Detectron2。

```
class SimplePyTorchModel(nn.Module):
    def __init__(self):
        super(SimplePyTorchModel, self).__init__()
        self.linear = nn.Linear(4, 1)
        self.linear.weight.data.fill_(0.01)
        self.linear.bias.data.fill_(0.01)

    def forward(self, X):
        return torch.relu(self.linear(X))
```

值得注意的是，我们无需训练模型，而是将权重和偏置固定为 0.01 以用于演示目的。以下代码片段随后创建了此简单神经网络的模型并将其置于评估模式：

```
pt_model = SimplePyTorchModel()
pt_model.eval()
```

现在，我们已准备好使用虚拟输入将模型导出为 ONNX 格式：

```
dummy_X = torch.tensor([[1, 2, 3, 4]], dtype=torch.float32)
model_name = 'onnx_model.onnx'
torch.onnx.export(pt_model,
    dummy_X, model_name, verbose=True)
```

要测试导出的模型，我们首先需要使用以下语句安装 onnx 包：

```
!pip install onnx
```

安装 onnx 后，以下代码片段加载导出的 ONNX 模型，检查其是否格式良好，并打印其图以供检查：

```
import onnx
from onnx.helper import printable_graph
# load
loaded_model = onnx.load(model_name)
# check (well formed)
onnx.checker.check_model(loaded_model)
# graph
print(printable_graph(loaded_model.graph))
```

要在 ONNX 模型中运行加载的模型，我们首先需要使用以下语句安装 onnxruntime 包：

```
!pip install onnxruntime
```

一旦运行时环境就绪，以下代码片段有助于创建推理会话并获取加载模型的输入名称：

```
import onnxruntime as ort
ort_session = ort.InferenceSession(model_name)
input_name = ort_session.get_inputs()[0].name
print(input_name)
```

之前的代码片段应将 input_name 显示为 onnx::Gemm_0。此输入名称对于在对输入数据进行推理时为加载的模型创建输入对象非常重要，如以下代码片段所示：

```
import numpy as np
X = np.array([[2, 3, 4, 5]])
outputs = ort_session.run(
    None,
    {input_name: X.astype(np.float32)},
)
print(outputs[0])
```

之前的代码片段创建一个批输入 X，其中包含一个实例 [2, 3, 4, 5] 并对该输入进行预测。此代码片段应显示结果为 [[0.15]]。为了验证预测的准确性，我们也可以使用原始 PyTorch 模型对同一输入进行预测，如下所示：

```
X = torch.tensor([[2, 3, 4, 5]], dtype=torch.float32)
with torch.no_grad():
    y = pt_model(X)
    print(y)
```

此代码片段应显示与在 ONNX 推理会话中执行的加载 ONNX 模型相同的输出（[[0.15]]）。

可选地，如果你正在 Google Colab 上运行，可以运行以下代码片段以将此 ONNX 模型下载以供将来使用：

```
```python
from google.colab import files
files.download('onnx_model.onnx')
```
```

我们现在应该已将 PyTorch 模型导出为 ONNX 格式。此模型可以部署在支持 ONNX 的各种框架和平台中。在下一节中，我们将将其部署到浏览器环境。

## 将 ONNX 模型加载到浏览器

如果意图是将 PyTorch 或 Detectron2 模型加载到 Web 服务器环境中，则上一章中描述的技术就足够了。此外，我们还可以将 ONNX 模型部署到 Web 服务器环境中。然而，在许多情况下，可能需要将深度学习模型部署到 Web 浏览器环境中。因此，本节提供了一个将上一节导出的 ONNX 模型部署到 Web 浏览器环境的示例。为简单和一致起见，让我们使用 Google Colab 来执行我们的 Web 应用程序。

以下代码片段将上一节创建的 ONNX 模型下载到当前运行的 Google Colab 实例中：

```
```bash
!wget <url_to/Chapter13/onnx_model.onnx>
```
```

此外，为了在浏览器上执行 ONNX 模型，我们需要使用以下片段将 onnx.js 下载到当前文件夹：

```
```bash
!wget https://cdn.jsdelivr.net/npm/onnxjs/dist/onnx.min.js
```
```

下载完成后，我们可以使用 %%writefile 指令在当前文件夹中创建一个名为 load_onnx.html 的 HTML 文件。此 HTML 文件有助于加载下载的模型并在示例输入上执行模型以验证流程：

```
```html
%%writefile load_onnx.html
<html>

  <head> </head>

  <body>
    <div id="output"></div>
    <script src="onnx.min.js">
    </script>
    <script>
      const myOnnxSession = new onnx.InferenceSession();
```




```
myOnnxSession.loadModel("./onnx_model.onnx")
.then(() => {
    // 第一步：创建一个输入
    // 第二步：运行并更新输出div
});
</script>
</body>
</html>
```

该 HTML 文件首先加载下载好的 JavaScript 文件（onnx.js）。接着，它创建另一段 JavaScript 来新建一个 ONNX 推理会话并加载下载好的 ONNX 模型（onnx_model.onnx）。模型（异步）加载完成后，会执行以下两个步骤。

第一步：这一步创建一个示例输入，作为一个 ONNX 张量，其数据与上一节中的相同（[2, 3, 4, 5]）：

```
const inferenceInputs = new onnx.Tensor(
  [2, 3, 4, 5],
  'float32', [1, 4]
);
```

第二步：这一步将上一步准备好的输入传给推理会话，以异步执行预测。执行生成输出对象后，它会取出输出数据并更新输出 div 的 innerText：

```
myOnnxSession.run([inferenceInputs])
.then((output) => {
  const outputTensor = output.values().next().value;
  const msg = `model output tensor: ${outputTensor.data}`;
  document.getElementById('output').innerText = msg;
});
```

下面的代码片段创建了一个辅助方法（expose_content），它可以将指定端口上运行的 web 服务器的内容暴露给 iframe，并将生成的 iframe 嵌入当前 Google Colab 的输出单元格中：

```
from IPython.display import Javascript
def expose_content(port, height=200):
    display(Javascript("""
(async () =>{
```

访问 D2Go 安装文件夹时请注意 Python 版本（python3.9）。如果安装目录中不存在 configs 文件夹，我们可以使用以下语句克隆 d2go 仓库：

```
!git clone <url_to/d2go.git> d2go_repo
```

克隆该仓库后，我们可以使用以下语句在安装目录中创建一个名为 configs 的文件夹：

```
d2go_dir = "/usr/local/lib/python3.9/dist-packages/d2go"
configs_dir = f"{d2go_dir}/configs"
import os
os.makedirs(configs_dir, exist_ok=True)
```

现在，我们可以将下载仓库中 configs 文件夹的内容复制过来：

```
!cp -r d2go_repo/configs {d2go_dir}
```

我们可以列出该文件夹中的所有配置文件以确认复制过程，同时还可以查看 D2Go Model Zoo 中已有的模型：

```
!ls "/usr/local/lib/python3.9/dist-packages/d2go/configs"
```

该语句应输出以下配置文件：

```
faster_rcnn_fbnetv3a_C4_FSDP.yaml
keypoint_rcnn_fbnetv3a_dsmask_C4.yaml
faster_rcnn_fbnetv3a_C4_LSJ.yaml
mask_rcnn_fbnetv3a_C4.yaml
faster_rcnn_fbnetv3a_C4.yaml
mask_rcnn_fbnetv3a_dsmask_C4.yaml
faster_rcnn_fbnetv3a_dsmask_C4.yaml
mask_rcnn_fbnetv3g_fpn.yaml
faster_rcnn_fbnetv3g_fpn.yaml
qat_faster_rcnn_fbnetv3a_C4.yaml
```

此外，从 d2go.model_zoo 包中导入 model_zoo 时可能会出现如下错误信息：

```
ModuleNotFoundError: No module named 'torch.ao.pruning'
```

如果出现该错误，你可以执行以下步骤作为此问题的快速修复：

- 1. 在 d2go 安装目录中找到 `fsdp.py` 文件（例如，在 Google Colab 中，路径为 `/usr/local/lib/python3.9/dist-packages/d2go/trainer/fsdp.py`）。
- 2. 找到第 16 行，将 `fqn_to_module` 的导入路径从 `'from torch.ao.pruning import fqn_to_module'` 改为 `'from torch.ao.sparsity.sparsifier.utils import fqn_to_module'`。
- 3. 按 Ctrl + S 键保存更改。

完成这些快速修复后，我们就可以在 D2Go 的 Model Zoo 中选择一个现有的 D2Go 模型。读取和选择 D2Go 预训练模型的方式与第 2 章中为 Detectron2 选择预训练模型的方式相同。让我们选择一个简单模型来演示加载 D2Go Model Zoo 上现有模型的步骤：

```
from d2go.model_zoo import model_zoo
selected_model = 'faster_rcnn_fgnetv3a_C4.yaml'
model = model_zoo.get(selected_model, trained=True)
```

完成此步骤后，我们应该已经加载了模型（model）。让我们准备一个简单输入并测试加载的模型：

```
!wget -qq <url_to/800x800image/input.jpeg>
```

下面的代码片段在推理前加载下载好的图像：

```
import cv2
img = cv2.imread("./input.jpeg")
```

与 Detectron2 中的 DefaultPredictor 类类似，D2Go 也提供了一个名为 DemoPredictor 的类，用于从加载的模型生成预测器。下面的代码片段根据输入的模型创建该类的一个实例，对加载的图像（img）执行推理并打印结果：

```
from d2go.utils.demo_predictor import DemoPredictor
predictor = DemoPredictor(model)
outputs = predictor(img)
pred_instances = outputs["instances"]
print(pred_instances)
```

该片段应输出以下结果：

```
Instances(num_instances=10, image_height=800, image_width=800,
fields=[pred_boxes: Boxes(tensor([[187.2431, 211.5772,
384.4924, 711.0326],
        [347.6918, 385.5148, 480.2947, 747.8323],
        [375.6132, 371.9870, 620.2853, 725.2900],
        [348.4673, 643.8885, 452.5689, 765.8668],
        [455.8702, 422.2232, 640.2230, 685.9368],
        [319.8739, 275.5755, 417.1956, 651.1306],
        [224.1623, 656.9628, 340.6416, 790.6293],
        [496.0885, 670.5968, 641.2301, 750.6605],
        [345.4467, 654.7966, 552.8320, 770.0833],
        [499.2968, 669.8000, 639.1102, 750.0955]])), scores:
tensor([0.9974, 0.9540, 0.9063, 0.6769, 0.4782, 0.3086, 0.2689,
0.1755, 0.0990,
        0.0933]), pred_classes: tensor([ 0,  0,  0, 30, 33,  0,
30, 30, 30, 31]))
```

可以看出，所有检测到的对象都被列出，无论预测置信度如何。让我们定义一个阈值（thres_test = 0.5）并使用该值过滤预测结果：

```
thresh_test = 0.5
selected_idxs = pred_instances.scores > thresh_test
pred_instances = pred_instances[selected_idxs]
pred_instances
```

该片段会过滤出具有较高预测置信度的实例：

```
Instances(num_instances=4, image_height=800, image_width=800,
fields=[pred_boxes: Boxes(tensor([[187.2431, 211.5772,
384.4924, 711.0326],
        [347.6918, 385.5148, 480.2947, 747.8323],
        [375.6132, 371.9870, 620.2853, 725.2900],
        [348.4673, 643.8885, 452.5689, 765.8668]])), scores:
tensor([0.9974, 0.9540, 0.9063, 0.6769]), pred_classes:
tensor([ 0,  0,  0, 30]))
```

让我们可视化输出并定量评估预测结果：

```
from detectron2.utils.visualizer import Visualizer
```

## 训练自定义 D2Go 模型

让我们使用脑肿瘤数据集训练一个 D2Go 模型，以执行脑肿瘤检测和分类任务。与上一节类似，我们需要安装 detectron2、mobile-vision 和 d2go 包。我们还必须将 configs 文件夹复制到 d2go 安装目录，并更改导入 fqn_to_module 的那行代码。这些步骤与上一节描述的相同。

我们还需要下载脑肿瘤数据集，并将训练集和测试集注册到 Detectron2，就像我们在训练 Detectron2 模型时经常做的那样（代码相同）。下载数据集并注册后，让我们选择一个 D2Go 模型进行训练。对于此任务，D2Go 提供了一个名为 GeneralizedRCNNRunner 的类，用于帮助创建配置文件并训练自定义模型。让我们先导入该类、选择一个模型并创建该 runner 类的一个实例：

```
from d2go.model_zoo import model_zoo
from d2go.runner import GeneralizedRCNNRunner
selected_model = "faster_rcnn_fbnetsv3a_C4.yaml"
nc = 2
output_dir = './output'
model_url = selected_model
runner = GeneralizedRCNNRunner()
```

现在，在开始训练之前，我们需要获取该模型的配置：

```
cfg = runner.get_default_cfg()
cfg.merge_from_file(model_zoo.get_config_file(model_url))
cfg.MODEL.EMA.ENABLED = False
cfg.DATASETS.TRAIN = (name_ds_train,)
cfg.DATASETS.TEST = (name_ds_test,)
cfg.DATALOADER.NUM_WORKERS = 2
cfg.MODEL.WEIGHTS = model_zoo.get_checkpoint_url(model_url)
cfg.MODEL.DEVICE ="cpu" if ('CI' in os.environ) else "cuda"
cfg.SOLVER.IMS_PER_BATCH = 2
cfg.SOLVER.BASE_LR = 0.00025
cfg.SOLVER.MAX_ITER = 5 if ('CI' in os.environ) else 5000
cfg.SOLVER.STEPS = []
cfg.MODEL.ROI_HEADS.BATCH_SIZE_PER_IMAGE = 128
cfg.MODEL.ROI_HEADS.NUM_CLASSES = nc
cfg.OUTPUT_DIR = output_dir
os.makedirs(cfg.OUTPUT_DIR, exist_ok=True)
```



大多数配置参数与 Detectron2 模型相同：创建配置、将其与现有配置文件合并以及下载预训练权重。我们现在可以使用配置（cfg）构建模型并使用脑肿瘤数据集训练该模型：

```
model = runner.build_model(cfg)
runner.do_train(cfg, model, resume=False)
```

显然，与训练 Detectron2 模型相比，训练此模型所需的时间要少得多，因为它是一个轻量级模型。让我们在测试数据集上评估训练好的模型：

```
metrics = runner.do_test(cfg, model)
print(metrics)
```

由于训练过程中的随机性，输出结果可能每次运行都不同。然而，在交并比（Intersection over Union，IoU）下的平均精度（Average Precision，AP）应约为 0.48，这是一个相当不错的评估结果。我们可以使用与上一节类似的代码对一些样本数据进行预测并可视化结果，以定性评估训练模型的性能。

该训练模型轻量级，旨在部署到移动环境中。然而，它仍然使用 32 位浮点数来存储其权重和激活值；因此，它仍然被认为效率低下。如果需要进一步压缩模型空间，模型量化技术就派上用场了。下一节将详细讨论该技术。

## 模型量化

模型量化是一种有助于在精度略有损失的情况下降低深度神经网络计算复杂度和内存占用的技术。具体来说，深度学习模型通常以 32 位浮点数的形式存储其权重和激活值，而这种高精度数字格式需要大量的数据存储和计算时间。因此，模型量化通过将权重和激活值转换为较低精度的数据类型（如 8 位整数）来解决此问题。这种降低有助于减少内存使用和计算时间。值得注意的是，虽然量化可以显著降低模型的内存和计算需求，但也可能导致模型精度的损失。

延续上一节，让我们先通过以下代码片段打印出训练模型第一个参数的数据类型：

```
for name, param in model.named_parameters():
    if param.requires_grad:
        print(name, param.data.dtype)
        break
```

## 总结

本章介绍了 ONNX 模型格式以及支持它的平台和框架。该框架有助于使 Detectron2 模型能够与不同框架和平台互操作。它随后提供了将 Detectron2 模型导出到该格式的步骤以及在浏览器环境中部署导出模型的代码。本章还介绍了 D2Go，这是一个用于以最小的内存存储和计算资源训练、优化和部署计算机视觉应用神经网络的框架。此外，其模型可进一步使用量化技术进行优化，该技术将模型权重和激活值转换为低精度数字系统。这种量化步骤进一步降低了模型内存需求并提高了计算性能。因此，D2Go 模型适合部署到移动或边缘设备。D2Go 在其 Model Zoo 上也有预训练模型。因此，本章提供了使用 D2Go Model Zoo 上现有模型构建计算机视觉应用的步骤。它还提供了使用 D2Go 工具训练自定义 D2Go 模型并执行量化的步骤。

恭喜，并感谢您跟随到最后一章！您现在应该认为自己是使用 Detectron2 开发实用计算机视觉应用的专家。此外，本书中提供的理论应有助于您探索其他计算机视觉深度学习架构，例如 **You Only Look Once (YOLO)**。
