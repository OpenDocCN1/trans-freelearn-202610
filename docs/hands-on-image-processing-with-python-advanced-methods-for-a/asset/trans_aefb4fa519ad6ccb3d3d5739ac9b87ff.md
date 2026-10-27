

接下来代码首先从 ADE20K 和 Cityscapes 加载基准图像，为基于 OneFormer 的通用分割准备输入。它使用 OneFormerProcessor 进行预处理（调整大小、归一化、标记化）和 OneFormerForUniversalSegmentation 进行基于变换器的推理。函数 `segment_image()` 执行语义分割，生成彩色掩码和叠加可视化，并显示最终的分割结果。

```python
from transformers import OneFormerProcessor, \
                        OneFormerForUniversalSegmentation

ade20k_image = Image.open('images/ADE_val_00000519.png').convert('RGB')
cityscapes_image = Image.open(\
    'images/stuttgart_00_000000_000113_leftImg8bit.png').convert('RGB')

ade_processor = OneFormerProcessor.from_pretrained(\
                        "shi-labs/oneformer_ade20k_swin_large")
ade_model = OneFormerForUniversalSegmentation.from_pretrained(\
                        "shi-labs/oneformer_ade20k_swin_large").to(device).eval()
city_processor = OneFormerProcessor.from_pretrained(\
                        "shi-labs/oneformer_cityscapes_swin_large")
city_model = OneFormerForUniversalSegmentation.from_pretrained(\
                        "shi-labs/oneformer_cityscapes_swin_large").to(device).eval()

def segment_image(image, processor, model, device):
    inputs = {k: v.to(device) for k, v in processor(image, ["semantic"],
                        return_tensors="pt").items()}
    with torch.no_grad():
        seg_map = processor.post_process_semantic_segmentation(
            model(**inputs), target_sizes=[image.size[::-1]])[0].cpu()
        # Plot image, seg_map and overlay side-by-side here

segment_image(ade20k_image, processor=ade_processor, model=ade_model,
              device='cuda')
```

如果运行上述代码对 ADE20K 中的图像进行分割，你应该会得到如下图所示的结果：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_544_0.png)

> *图 10.34 – 使用预训练 OneFormer 分割 ADE20k 图像*

同样，要对 Cityscapes 中的图像进行分割，请运行以下代码：

```python
segment_image(cityscapes_image, processor=city_processor, model=city_model)
```

如果运行上述代码，你应该会得到如下图所示的结果：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_545_0.png)

> *图 10.35 – 使用预训练 OneFormer 分割 Cityscape 图像*

上述代码演示了真实世界的数据集，介绍了标准化的评估基准，并为通用分割流程奠定了基础。OneFormer 学会了在任何任务下、在任何地方分割任何东西，使用单一统一的变换器。

要点：最后一部分将经典的、基于 CNN 的和基于变换器的分割统一在共同的视角下，突出了方法如何从像素/区域建模演进到以对象为中心和基于查询的表示。它强调了语义、实例和全景分割在现代架构中的核心区别和联系。

## Summary

在本章中，我们建立了图像分割的坚实基础，从经典概念发展到现代深度学习和基于变换器的方法。我们探讨了语义、实例和全景分割的基本公式，并研究了诸如 CNN 和变换器（例如 DPT、SegFormer、DETR、Mask2Former）等架构如何建模空间上下文、全局依赖和对象级推理。通过理论见解和实际实现，读者对分割模型如何从像素级分类演进到统一的、基于查询的框架有了清晰的理解。这一基础为我们进入下一部分做好了准备，在那里分割将变得更加交互式、多模态和以应用为驱动。

在第 11 章《图像分割的更多深度学习方法》中，我们将探索可提示的基础模型、视觉-语言和文本引导分割、单目深度估计和 3D 场景理解，以及用于真实世界分割任务的先进架构和实用流程。

## Get this book’s PDF version and more

扫描二维码（或访问 packtpub.com/unlock）。按书名搜索此书，确认版本，然后按照页面上的步骤操作。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_546_0.png)

UNLOCK NOW

> Note: Keep your invoice handy. Purchases made directly from Packt don’t require an invoice.

## 11 More Deep Learning Methods for Image Segmentation

在上一章讨论的经典和基于深度学习的分割技术的基础上，本章继续探索下一代深度学习方法，强调灵活性、泛化能力和真实世界部署。我们首先介绍 SAM 和 MaskCLIP 等可提示基础模型，它们能够通过最少的用户输入或文本指导实现分割，然后介绍视觉-语言和基于扩散的方法，将语义与生成建模连接起来。

本章随后转向实际应用，演示分割如何在背景操纵和工业级框架中的对象检测等任务中使用。特别关注训练自定义模型，包括 U-Net 变体和现代骨干网络，进一步扩展到医学成像（MRI、CT）和多源卫星数据的遥感等特定领域应用。最后，我们讨论 3D 分割和可视化，以及对评估指标（IoU、Dice、mAP）的严格处理，使读者具备设计、训练和评估跨不同真实场景分割系统的理论理解和实用工具。

在本章中，我们将学习以下关键主题：

- Promptable foundation models for image segmentation
- Depth estimation and 3D scene understanding from a single image
- Practical applications of image segmentation
- Training a custom segmentation model
- 3D segmentation of liver CT images with Swin-UNETR
- Remote sensing: Multi-source satellite imagery segmentation with mixed-attention UNet
- Segmentation evaluation metrics

## Technical requirements

在阅读本章不同部分的同时，请跟随 Jupyter notebook 并运行必要的代码片段。以下是对应本章笔记本的 GitHub 链接：https://github.com/PacktPublishing/Hands-On-Image-Processing-and-Computer-Vision-with-Python-Second-Edition。在运行代码之前，必须安装正确版本的库才能无缝运行。如果版本不匹配，代码可能需要根据库中某些功能/函数是否被移除或弃用进行少量调整。

## Promptable foundation models for image segmentation

传统分割模型被训练为将图像 $I \in \mathbb{R}^{H \times W \times 3}$ 直接映射到密集标签图 $S: \Omega \subset \mathbb{R}^2 \to \mathcal{C}$，其中 $\mathcal{C}$ 是固定的类别集合。相比之下，**promptable foundation models** 将分割重新定义为 **条件预测问题**，输出不仅取决于图像，还取决于外部 **prompt** $p$：

$$S(x, y \mid p) = f_\theta(I, p),$$

其中提示 $p$ 可以采用多种形式：稀疏点、边界框、文本描述和粗略掩码。这种范式引入了 **交互式、零样本和可泛化分割**，其中单一模型可以在无需重新训练的情况下分割任意对象。

### Segment Anything Model

**Segment Anything Model (SAM)** 是一个在大规模掩码上训练的大规模视觉基础模型。它使用点、框或掩码作为指导执行 **prompt-driven segmentation**。SAM 将分割分解为三个组件：

- Image encoder: $F = E_I(I), \quad F \in \mathbb{R}^{N \times D}$
- Prompt encoder: $P = E_p(p)$
- Mask decoder: $M = D(F, P)$

最终预测：

$$M(x, y) = \sigma\left(f_\theta(I, p)\right),$$

其中 $\sigma$ 是产生二值掩码的 sigmoid 函数。因此，SAM 学习的是以提示为条件的通用分割函数，而非固定类别。

下一个代码片段演示了使用 SAM 在 GPU 上进行高效的自动图像分割。预训练的 ViT-Huge SAM 模型从 `sam_model_registry` 加载，采用 FP16 精度以实现更快的 CUDA 推理，并且 `SamAutomaticMaskGenerator()` 通过采样图像点并使用 IoU 和稳定性标准进行过滤来生成候选分割掩码。

