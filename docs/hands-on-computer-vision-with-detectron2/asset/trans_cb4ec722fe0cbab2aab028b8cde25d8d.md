

对于配置文件接下来的几段代码，理想情况下，最好运行一些步骤来为学习率、像素均值和标准差以及锚点大小和比例生成不同的配置。然而，为了简单起见，我们复用了之前执行的结果。此外，PointRend 模型需要为其 Point Head 设置类别数量；下面的代码片段设置了该配置：

```
cfg.MODEL.POINT_HEAD.NUM_CLASSES = nc
```

一旦我们有了必要的配置，就可以开始训练模型了。用于创建训练器类（BrainTumorTrainer）、最佳模型钩子（BestModelHook）、创建训练器以及注册钩子的代码片段与前一章中解释的源代码保持一致。

在撰写本书时，当前的 PointRend 项目在训练过程中，如果某个训练批次没有产生任何实例（空的实例列表），就会抛出错误。这个问题在大多数数据集上不会出现，但在某些数据集上会出现。例如，在脑肿瘤分割数据集上训练当前模型时，由于批次大小较小，就会遇到这个问题。因此，下面的代码片段修改了 PointRend 内部的采样策略代码以处理该异常。下面的代码片段导入了完成此任务所需的包：

```
import torch
from detectron2.layers import cat
from detectron2.structures import Boxes
from detectron2.projects.point_rend.point_features import (
    get_point_coords_wrt_image,
    sample_point_labels,
    get_uncertain_point_coords_with_randomness
)
from detectron2.projects.point_rend.mask_head import (
    calculate_uncertainty
)
```

下面的代码片段重新定义了 `_sample_train_points` 方法来处理该异常：

```
def _sample_train_points(self, coarse_mask, instances):
    assert self.training
    gt_classes = cat([x.gt_classes for x in instances])
    with torch.no_grad():
        # Step 1: sample point coordinates (point_coords)
        # Step 2: get point_coords with respect to the image
```

![](img/0e7994c850a84b7a53d34c7f943c7edd_260_0.png)

![](img/0e7994c850a84b7a53d34c7f943c7edd_260_1.png)

真实分割掩膜

使用 PointRend 的预测分割掩膜

图 11.5：真实分割掩膜与使用 PointRend 的预测结果对比

可以观察到，使用 PointRend 的预测分割（右侧）相比真实分割具有更精确的边界。

恭喜！至此，你应该已经深入理解了 PointRend 项目以及它如何帮助提升目标实例分割的质量。此外，你可以使用现有的 PointRend 模型，或在自定义数据集上训练 PointRend 模型，来开发目标实例分割应用。

## 总结

本章介绍了微调使用 Detectron2 训练的目标实例分割应用的技术。总的来说，目标实例分割应用也使用目标检测部分。因此，所有用于微调目标检测模型的方法都可以用于目标实例分割模型。此外，本章讨论了 PointRend 项目，它有助于改进检测到的对象的目标实例分割边界。具体来说，本章描述了 PointRend 的工作原理，以及使用 PointRend 模型库中现有模型开发目标实例分割应用的步骤。最后，本章还提供了在自定义数据集上训练自定义 PointRend 模型的代码片段。

再次恭喜！至此，你应该已经深入掌握了 Detectron2 的知识，并且能够通过使用现有模型或在自定义数据集上训练自定义模型来开发计算机视觉应用。下一章将讨论将构建好的模型投入部署的步骤。

## 第 4 部分：
将 Detectron2 模型部署到生产环境

最后这部分将引导你完成导出过程的各个步骤，将 Detectron2 模型转换为可部署的产物。具体来说，它描述了深度学习模型的标准文件格式（如 TorchScript）以及这些格式对应的运行时（如 PyTorch 和 C++ 运行时）。然后，它提供了将 Detectron2 模型转换为标准文件格式并部署到相应运行时的步骤。此外，这部分还介绍了开放神经网络交换（ONNX）框架。该框架有助于在多个框架和平台之间共享深度神经网络。当需要将 Detectron2 模型部署到浏览器或移动环境中时，它极其有用。最后，这部分介绍了 D2Go，这是一个用于训练、量化和部署模型的框架，具有极低的内存存储和计算需求。使用该框架创建的模型对于部署到移动或边缘设备非常有用。

第四部分涵盖以下章节：

- 第 12 章，将 Detectron2 模型部署到服务器环境
- 第 13 章，将 Detectron2 模型部署到浏览器和移动环境

## 12

## 将 Detectron2 模型部署到服务器环境

本章将引导你完成导出过程的步骤，将 Detectron2 模型转换为可部署的产物。具体来说，它描述了深度学习模型的标准文件格式（如 TorchScript）以及这些格式对应的运行时（如 PyTorch 和 C++）。然后，本章提供了将 Detectron2 模型转换为标准文件格式并部署到相应运行时的步骤。

学完本章后，你将理解 Detectron2 支持的标准文件格式和运行时。你可以执行步骤，使用跟踪（tracing）或脚本化（scripting）方法将 Detectron2 模型导出为 TorchScript 格式。此外，你可以创建一个 C++ 应用程序来加载和执行导出的模型。

在本章中，我们将涵盖以下主题：

- PyTorch 模型支持的文件格式和运行时
- 部署自定义 Detectron2 模型

## 技术要求

你应该已经完成第 1 章的学习，以具备合适的 Detectron2 开发环境。所有代码、数据集和结果都可以在本书的 GitHub 页面上找到：https://github.com/PacktPublishing/Hands-On-Computer-Vision-with-Detectron2。

## 支持的文件格式和运行时

本节介绍 PyTorch 应用（尤其是 Detectron2 应用）的开发环境、文件格式和运行时。然后使用简单示例来解释这些概念。

## 开发环境、文件格式和运行时

在开发和部署深度学习模型时，有三个主要概念需要掌握：开发环境和编程语言、模型文件格式以及运行时环境。图 12.1 展示了这三个概念及各自的示例。

![](img/0e7994c850a84b7a53d34c7f943c7edd_265_0.png)

图 12.1：开发环境、文件格式和部署环境

开发环境和编程语言或框架（如 PyTorch 或 Python）通常优先考虑易用性和定制性。然而，Python 语言和 PyTorch 框架在部署时可能会遇到运行时问题。例如，它们相对于其他运行时环境（如由 C++ 应用程序生成的可执行文件）来说速度较慢。此外，它们无法直接部署到 Web 浏览器或移动设备上。因此，用 PyTorch 编写的模型通常需要经过导出过程才能成为可部署的产物。

用 PyTorch 编写的模型有其自身的文件格式，并且旨在通过 PyTorch 运行时执行。然而，为了支持跨平台以及与其他运行时环境的兼容性，PyTorch 支持另一种称为 TorchScript 的中间文件格式。这种 TorchScript 文件可以在不同的运行时环境中执行，例如由 C++ 应用程序加载以生成可执行文件。PyTorch 还允许将其模型导出为开放神经网络交换（ONNX）文件格式，以支持其他运行时环境，如 Web 浏览器和移动设备。

TorchScript 有自己的解释器，其功能类似于 Python 的受限版本。这种格式使我们能够将整个模型保存到磁盘，并在不同的环境中使用，例如使用 Python 以外的其他语言构建的服务器。TorchScript 格式允许编译器优化，从而带来更高效的执行。此外，TorchScript 还可以与多种后端/设备运行时交互。

Detectron2 支持使用跟踪（tracing）和脚本化（scripting）方法将其模型导出为 TorchScript。它还允许用户将模型导出为 ONNX 文件格式，该格式可以加载到不同的运行时环境中，如 Web 浏览器和移动设备。Detectron2 模型可以使用 caffe2_tracing 方法导出为 Caffe2 文件格式。然而，Caffe2 即将被弃用并与 PyTorch 合并。因此，本书不考虑这种选项。最后，在撰写本书时，TensorFlow 在生产环境中比 PyTorch 更受欢迎。因此，Detectron2 支持将其模型导出为 TensorFlow 的文件格式。

