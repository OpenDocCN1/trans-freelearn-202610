

Detectron2 提供了一个名为 scripting_with_instances 的函数，用于使用 PyTorch 脚本化函数（torch.jit.script）执行导出。Detectron2 模型使用 Instances 类，并且该类的属性可以在急切模式下动态添加。PyTorch 脚本化函数不支持这种动态行为。这就是为什么需要此辅助函数（scripting_with_instances）。它接受以下输入参数：

- model：这是使用脚本化方法导出的 Detectron2 模型
- fields：它给出模型中 Instances 类使用的属性名称和类型

此方法创建一个名为 new_Instances 的类，与 Instances 类似但具有静态属性。静态属性必须使用辅助方法的 fields 输入参数声明。new_Instances 类使模型可追踪。以下是此辅助函数的示例用法：

```
fields = {
    "proposal_boxes": Boxes,
    "objectness_logits": Tensor,
    "pred_boxes": Boxes,
    "scores": Tensor,
    "pred_classes": Tensor,
    "pred_masks": Tensor,
    "pred_keypoints": Tensor,
    "pred_keypoint_heatmaps": Tensor,
}
model.eval()
scripted_model = scripting_with_instances(model, fields)
```

恭喜！你现在了解了帮助使用追踪和脚本化方法导出 Detectron2 模型的两个主要工具。让我们将这些方法应用于之前构建的自定义 Detectron2 模型的导出。

## 导出自定义 Detectron2 模型

我们首先需要安装 Detectron2，加载数据集，并获取用于导出的训练好的模型及其配置文件：

```
# install
!python -m pip install \
'git+https://github.com/facebookresearch/detectron2.git'
# dataset
```

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
