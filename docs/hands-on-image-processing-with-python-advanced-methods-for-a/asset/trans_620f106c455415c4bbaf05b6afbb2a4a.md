

总损失为：

$$L_{total} = \lambda_{spa} L_{spa} + \lambda_{exp} L_{exp} + \lambda_{col} L_{col} + \lambda_{tv} L_{tv}$$

所提出的网络 DCE-Net 轻量且为全卷积结构，由七个卷积层、ReLU 激活和跳跃连接构成，以保留空间信息。

**输入：** RGB 低光图像

**输出：** 用于 α_t 的 3 × n 通道堆栈，针对 n 次迭代的每个 3 个颜色通道。

最终层使用 tanh 激活以约束输出 α_t ∈ [−1,1]。

对于前向传递逻辑，网络预测 n 个单独的 α_t 映射，每个映射有 3 个通道。这些映射沿通道轴拆分以迭代应用增强曲线。迭代地使用增强曲线细化图像 x。如有需要存储中间结果（例如第 4 次和第 8 次迭代）。

以下 Python 代码演示了如何使用预训练模型增强几张低光图像。它首先将预训练权重加载到实例化的 DCENet 模型对象中，然后将模型移至 GPU 以加速。对于增强图像目录，它加载并归一化每个输入图像，将其转换为张量并移至 CUDA，然后应用模型并保存增强后的输出。

### 为什么这会有效？

- 增强曲线是逐像素自适应的，通过 CNN 学习
- 迭代应用允许渐进细化
- 损失项鼓励曝光校正、色彩平衡和结构保留，而无需监督
- 网络紧凑（少于 100k 参数），在 GPU 甚至移动设备上运行速度快

现在，参考以下代码片段：

```
import torch, torchvision, torch.nn as nn
from pathlib import Path

class DCENet(nn.Module):
    def __init__(self, n=8, return_results=[4, 6, 8]):
        super().__init__()
        self.n = n
        self.ret = return_results
        self.conv1 = nn.Conv2d(3, 32, kernel_size=3, padding=1, bias=True)
        self.conv2 = nn.Conv2d(32, 32, kernel_size=3, padding=1,
            bias=True)
        self.conv3 = nn.Conv2d(32, 32, kernel_size=3, padding=1,
            bias=True)
        self.conv4 = nn.Conv2d(32, 32, kernel_size=3, padding=1,
            bias=True)
        self.conv5 = nn.Conv2d(64, 32, kernel_size=3, padding=1,
            bias=True)
        self.conv6 = nn.Conv2d(64, 32, kernel_size=3, padding=1,
            bias=True)
        self.conv7 = nn.Conv2d(64, 3 * n, kernel_size=3, padding=1,
            bias=True)
        self.relu = nn.ReLU(inplace=True)
        self.tanh = nn.Tanh()

    def forward(self, x):
        out1 = self.relu(self.conv1(x))
        out2 = self.relu(self.conv2(out1))
        out3 = self.relu(self.conv3(out2))
        out4 = self.relu(self.conv4(out3))
        out5 = self.relu(self.conv5(torch.cat((out4, out3), 1)))
        out6 = self.relu(self.conv6(torch.cat((out5, out2), 1)))
        alpha_stacked = self.tanh(self.conv7(torch.cat((out6, out1), 1)))
        alphas = torch.split(alpha_stacked, 3, 1)
        results = [x]
        for i in range(self.n):
            x = x + alphas[i] * (torch.pow(x, 2) - x)
            if i + 1 in self.ret: results.append(x)
        return results, alpha_stacked

device = torch.device('cuda:0') # Run on GPU (e.g., google colab)
model = DCENet(return_results=[4, 8])
model.load_state_dict(torch.load('models/8LE-color-loss2_best_model.pth',
                map_location='cuda:0', weights_only=False)['model'])
model.to(device)
```

```python
inPath = Path('images/low_light_inp')
outPath = Path('images/enhanced_out')
outPath.mkdir(parents=True, exist_ok=True)
num_images = 0
for file in inPath.glob('*'):
    img = Image.open(file)
    img = torch.from_numpy(np.array(img))
    img = img.float().div(255)
    img = img.permute((2, 0, 1)).contiguous()
    img = img.unsqueeze(0)
    img = img.to(device)
    results, Astack = model(img)
    enhanced_image = results[1]
    torchvision.utils.save_image(enhanced_image, \
        outPath.joinpath('enhanced_' + file.name))
    num_images += 1
```

如果运行上述代码片段，您应获得以下图：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_364_0.png)

*Figure 7.35 – Low-light image enhancement with Zero-DCE pretrained model*

## 使用多轴 MLP 的去噪

传统的 Gaussian 滤波、wavelet shrinkage 和 BM3D 方法在一定程度上已被证明有效，但基于深度学习的方法现在引领了最先进水平。其中一个模型是 **Multi-Axis MLP (MAXIM)**，由 Google Research 开发，它在真实去噪基准测试 **Smartphone Image Denoising Dataset (SIDD)** 上展示了令人印象深刻的性能。

MAXIM (Tao et al., 2022) 是一种通用图像恢复框架，采用新颖的架构，集成了：

- **MAMLP** 以实现全局感受野学习
- **局部和全局操作** 以捕捉精细和粗糙特征
- **具有跳跃连接的分层编码器-解码器架构**

MAXIM 代表了一类新的神经架构，利用 MLP 进行去噪等密集图像预测任务。它在 SIDD 和其他基准测试上的强劲性能证明了其现实世界的相关性。

MAXIM 架构亮点包括：

- **基于 MLP 的处理：** 与卷积架构不同，MAXIM 用多轴 MLP 层替代自注意力或卷积，以实现高效处理
- **交叉门控机制：** 有助于编码器和解码器特征的更好融合
- **多尺度分层解码器：** 逐步重建精细细节

MAXIM 变体（例如 MAXIM-S3）设计为轻量且快速，同时仍提供顶级性能。在这里，我们将使用 Google 在 Hugging Face 上提供的预训练 MAXIM-S3 模型进行真实图像去噪，如下一段代码所示。Keras 和 Hugging Face 的集成使 MAXIM 在 Python 项目中易于用于实际增强任务。

代码中的关键步骤如下：

- **加载预训练模型：** 使用函数 `from_pretrained_keras()` 通过 Hugging Face 加载在 SIDD 数据集上训练的 MAXIM-S3 模型
- **遍历噪声图像：** 处理指定噪声文件夹中的所有 .jpg 图像
- **预处理每张图像：** 调整图像大小并将其转换为批量为 1 的张量（使用 `tf.expand_dims()`）以供模型输入
- **运行 MAXIM 去噪：** 生成 3 个中间输出；第 3 个 (`denoised[0][2]`) 是最终去噪结果
- **后处理输出：** 裁剪值（使用 `tf.clip_by_value()`）并将输出张量转换回可显示的图像格式
- **并排可视化：** 显示噪声输入和去噪输出以进行视觉比较

现在参考下一段代码片段：

```
from huggingface_hub import from_pretrained_keras
import tensorflow as tf
# Load pretrained MAXIM for SIDD denoising
model = from_pretrained_keras("google/maxim-s3-denoising-sidd")
for imf in sorted(glob('images/noisy/*.jpg')):
    im = Image.open(imf)          # Load a sample noisy image
    im = im.resize((256, 256))
    im_np = np.array(im)
    im_tensor = tf.convert_to_tensor(im_np, dtype=tf.float32)
    im_tensor = tf.expand_dims(im_tensor, 0)  # batch = 1
    denoised = model.predict(im_tensor) # Run inference for denoising
    denoised[0][0].shape, denoised[0][1].shape, denoised[0][2].shape
    denoised_np = tf.clip_by_value(denoised[0][2].squeeze(), 0, 255).numpy().astype(np.uint8) # Squeeze and convert back to uint8
    # Display input image im_np vs. Output denoised_np here
```

如果运行上述代码片段，您应获得以下图：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_366_0.png)

*Figure 7.36 – Denoising images with a pretrained MAXIM model*

MAXIM 在以下方面表现良好：

- **长程依赖：** 传统 CNN 受局部感受野限制。MAXIM 的 MLP 层可以建模全局上下文。
- **多尺度监督：** 有助于在各种空间分辨率下细化细节。
- **交叉门控块：** 改善编码器和解码器特征之间的交互。

## 使用暗通道先验的去雾

户外摄影通常由于雾霾、雾气或大气散射导致可见度降低。雾霾会影响色彩保真度和对比度，使目标检测和场景理解更加困难。在本节中，我们将探讨一种强大且广泛使用的图像去雾方法，称为 **暗通道先验 (DCP)**。

该技术最初由 He、Sun 和 Tang 在 2009 年具有影响力的论文中提出，利用了关于无雾图像的一个简单而深刻的观察：无雾户外图像中的大多数局部块都包含至少一个颜色通道中强度非常低的像素。这种统计先验，即暗通道，构成了有效的去雾算法的基础。

雾化图像的形成通常由大气散射模型建模：

$$I(x) = J(x) \cdot t(x) + A \cdot (1 - t(x))$$

其中：

- $I(x)$：像素 $x$ 处的观测雾化图像
- $J(x)$：场景辐射度（待恢复的真实图像）
- $A$：全局大气光（通常为 3 通道 RGB 向量）
- $t(x)$：介质透射率描述到达相机的未被散射的光的比例

去雾的目标是在给定 $I(x)$ 的情况下恢复 $J(x)$。

### 暗通道先验

考虑图像 $I$ 的暗通道，其中

- $\Omega(x)$：以像素 $x$ 为中心的局部块
- $I^c(y)$：像素 $y$ 处颜色通道 $c$ 的强度

