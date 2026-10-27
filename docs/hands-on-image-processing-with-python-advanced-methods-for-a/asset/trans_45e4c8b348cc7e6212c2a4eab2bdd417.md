
在这里，预训练模型将其学到的视觉特征适配到车辆损伤分割任务上。在训练过程中，YOLO 联合优化定位、分类和分割掩码损失，以提升像素级的损伤预测能力。

**损伤分割与严重程度估计**：训练结束后，加载性能最佳的检查点。

```
best_model = YOLO("runs/segment/train-7/weights/best.pt")
```

在测试图像上执行推理：

```
results = best_model(test_image)
```

模型返回与检测到的损伤区域对应的分割掩码。通过统计掩码像素数量来估算总损伤面积。

```
masks = results[0].masks.data.cpu().numpy()
for mask in masks:
    area = np.sum(mask > 0.5)
    total_damage += area
```

数学上，如果 M(x, y) 表示二值损伤掩码，则 A = Σₓ,ᵧ M(x, y) 给出以像素为单位的损伤面积。随后，该面积被映射到严重程度类别：

```
if total_damage < 5000:    severity = "Minor"
elif total_damage < 20000:  severity = "Moderate"
else:                       severity = "Severe"
```

尽管方法较为简单，这种基于规则的方式展示了如何将分割输出转换为保险相关的量化指标。

**结果可视化**：预测掩码直接渲染在车辆图像上。

```
annotated = results[0].plot()
```

将原始图像与分割出的损伤区域进行并排对比，能够对预测质量进行直观验证。最终输出不仅定位了受损部件，还提供了估计的严重程度等级，展示了深度学习如何助力自动化保险检查、理赔处理、车队管理和车辆状况评估系统。

## 本章小结

本章全面介绍了现代图像分类与目标检测的完整历程，涵盖了基础的深度学习方法以及提升精度、效率、鲁棒性和数据效率的新兴范式。通过整合识别、定位、分割、文本提取和基于关键点的分析，这些方法使机器能够从视觉数据中提取有意义的信息，并支持广泛的现实应用。本章所建立的概念自然地为下一章铺平了道路——下一章的重点将从理解图像转向通过生成式 AI 和图像合成模型来创造图像。在下一章中，我们将探索计算机视觉领域最新的生成式 AI 模型。

## 订阅 Agentic Engineering，并在参与过程中免费获取一本电子书

如果你已经订阅，可以跳过这部分。你已经找到了志同道合的人。

如果没有，我们希望自从几章前我们首次介绍 **Agentic Engineering** 以来，你所读到的内容已经足以让你有理由再看一眼。不过不强求，只是温和地劝说你一下。

订阅后，我们将免费赠送一本 Valentina Alto 所著的 *AI Agents in Practice*。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_669_0.png)

https://packt.link/IQQG8

13

# 图像处理与计算机视觉中的生成式 AI

前一章聚焦于图像分类和目标检测，计算机视觉系统在其中学习识别和定位视觉内容。相比之下，本章探索快速发展的生成式 AI 领域，模型不仅要理解图像，还要学会创建、修改、修复和变换图像。本章从生成对抗网络和变分自编码器开始，逐步深入到基于扩散的方法（如 Stable Diffusion、SDXL、ControlNet 和 GPT 图像模型），并使用 Python 介绍现代图像合成、图像编辑、修复、风格迁移和多模态视觉系统的理论基础与实践实现。

本章首先介绍生成建模的基础概念，包括**生成对抗网络（GANs）**和**变分自编码器（VAEs）**，然后逐步推进到现代基于扩散的方法，涵盖 Stable Diffusion、SDXL、ControlNet 和 OpenAI GPT-Image 模型。本章还探讨了多模态视觉-语言系统、提示引导的图像生成、可控图像合成，以及用于真实世界计算机视觉和图像处理应用的实用 Python 实现。这些主题共同为理解那些将现代视觉 AI 从图像理解推向图像创造的技术提供了全面介绍。

在本章中，我们将学习以下主题：

- 使用条件生成对抗网络（cGANs）进行类别条件图像生成
- 使用条件变分自编码器（CVAE）进行条件图像生成
- Pix2Pix 和 CycleGAN 用于图像到图像的转换
- 基于 StyleGAN 的高保真人脸合成与语义操作
- GFPGAN：使用生成式人脸先验进行盲人脸修复
- 去噪扩散概率模型（DDPMs）
- 使用 Stable Diffusion 进行文本到图像的生成
- 使用 InstructPix2Pix 进行图像编辑
- 使用 ControlNet 进行结构引导的图像生成
- Stable Diffusion 和 SDXL 的实际应用
- 用于保持身份的姿态引导生成的 IP-Adapter 和 ControlNet
- 使用 DALL·E 2 和 DALL·E 3 API 进行图像生成
- GPT 图像 API：生成、编辑、修复和视觉

# 技术要求

在阅读本章各节时，请跟随 Jupyter notebook（并运行必要的代码片段）。以下是本章对应的 notebook 的 GitHub 链接：https://github.com/PacktPublishing/Hands-On-Image-Processing-and-Computer-Vision-with-Python-Second-Edition。为了使代码能够无缝运行，在运行代码之前，必须安装正确版本的库。如果版本不匹配，根据库中的某些特性/函数是否被移除或弃用，代码可能需要进行一些细微调整。

# 使用条件生成对抗网络（cGANs）进行类别条件图像生成

**生成对抗网络（GANs）**由 Ian Goodfellow 及其同事于 2014 年提出，是现代机器学习中最具影响力的生成模型之一。GAN 由两个相互竞争的神经网络组成：一个**生成器** *G*，从随机噪声中合成图像；一个**判别器** *D*，试图区分真实图像和生成图像。通过这种对抗博弈，生成器逐渐学会生成越来越逼真的样本。

原始的 GAN 学习数据分布 *p(x)*，但对生成何种类型的图像几乎无法控制。例如，在 *Fashion-MNIST* 上训练时，标准 GAN 可能生成衬衫、鞋子、连衣裙或包，但无法被指示合成特定类别。**条件生成对抗网络（cGANs）**通过将生成器和判别器都附加类别标签、属性或文本描述等额外信息作为条件，克服了这一限制。

**训练数据集**：*Fashion-MNIST* 是一个基准图像数据集，包含**10 个服装类别中的 70,000 张 28 × 28 灰度图像**（例如 T 恤、裤子、连衣裙、运动鞋和包），特别适合用于演示 cGAN，因为离散的类别标签能够清晰地展示**类别条件图像生成**，使模型能够按需合成特定服装类别，同时训练计算成本较低。

**条件对抗学习**：在 cGAN 中，生成器接收潜在向量 *z* 和条件 *y*：*x̂ = G(z, y)*。

类似地，判别器评估图像-标签对：*D(x, y)*。

目标函数变为：

min_G max_D 𝔼_{x ~ p_data}[log D(x, y)] + 𝔼_{z ~ p_z}[log(1 − D(G(z, y), y))]

因此，判别器不仅要判断图像是否逼真，还要判断图像是否与所提供的条件匹配。在本例中，y 对应于 Fashion-MNIST 的十个类别之一，使模型能够按需生成特定服装类别。

**生成器架构**：生成器对类别标签进行嵌入，将其与潜在向量拼接，将结果投影为特征张量，并将其上采样为 28 × 28 的 Fashion-MNIST 图像。

```
class Generator(nn.Module):
    def __init__(self):
        super().__init__()
        self.emb = nn.Embedding(NUM_CLASSES, 128)
        self.fc = nn.Sequential(
            nn.Linear(LATENT_DIM + 128, 7*7*256),
            nn.ReLU()
        )
        self.net = nn.Sequential(
            nn.ConvTranspose2d(256,128,4,2,1),
            nn.ReLU(),
            nn.ConvTranspose2d(128,64,4,2,1),
            nn.ReLU(),
            nn.Conv2d(64,1,3,1,1),
            nn.Tanh()
        )

    def forward(self, z, y):
        y = self.emb(y)
        x = torch.cat([z,y], dim=1)
        x = self.fc(x).view(-1,256,7,7)
        return self.net(x)
```

__init__() 方法通过定义网络层和可训练参数来构建网络架构，而 forward() 方法实现从输入到输出的前向传播过程中执行的计算。生成器首先使用 nn.Embedding() 将类别标签转换为可学习的嵌入表示。该嵌入捕获每个类别的语义信息，并将离散标签转换为连续的向量表示。然后，使用 torch.cat() 将潜在向量和类别嵌入拼接起来。数学上，

$$h = [z; e(y)]$$

这种组合表示被投影为低分辨率特征图，并使用转置卷积逐步上采样，将空间分辨率从 7 × 7 → 14 × 14 → 28 × 28 逐步提高。最后，Tanh 激活函数生成归一化的灰度图像。

**投影判别器**：一种朴素的 conditioning 方法是将标签与图像特征拼接。现代 cGAN 通常使用投影判别器，它提供更强的条件约束。判别器提取图像特征并计算投影分数：

$$h(x)^T e(y)$$

