
下面的代码片段将在多源卫星影像图块上训练一个 MANet 模型，以执行多类别土地覆盖分割，使用加权交叉熵和 Dice 损失，并结合数据增强与分块推理。

## 数据准备与训练策略

以下实现流程通过基于图块的训练、标签编码、类别平衡策略和数据增强来应对这些挑战，从而实现对大型、异质卫星影像的稳健分割：

- 1. **基于图块的训练：** 为了克服内存限制，将大尺寸卫星影像划分为较小的图块（PATCH=512）。函数 `extract_patches()` 从 RGB 图像和分割掩码中提取大小为 512 × 512 的非重叠图块。

数学上，

$$P_{ij}=I[x_i:x_i+P, y_j:y_j+P],$$

其中 P = 512 表示图块大小。这样可将训练数据集转换为成千上万个更小的样本（图块总数：5318）。基于图块的训练具有多项优势，例如降低 GPU 内存消耗、增大数据集规模、提升随机多样性，以及更容易进行小批量优化。

- 2. **基于颜色的掩码编码：** 数据集将语义掩码存储为 RGB 彩色图像，而不是整数标签。例如：

| RGB 颜色 | 语义类别 |
|---|---|
| (0,255,0) | 植被 |
| (255,0,0) | 建筑物 |
| (0,0,255) | 水体 |

下面的代码片段会构建一个映射（`COLOR_MAP`）。编码操作将 RGB 掩码转换为类别标签张量：

$$M(x,y) \in \{0,1,\ldots,C-1\}$$

转换过程使用：`label[(mask==c).all(axis=-1)] = i`。这会生成适合神经网络训练的整数值分割掩码。

- 3. **类别不平衡补偿：** 使用中值频率平衡来处理类别不平衡问题。

设 $f_c = \frac{N_c}{\sum_k N_k}$ 表示类别 $C$ 的频率，其中 $N_c$ 是属于类别 $C$ 的像素数。

类别权重定义为：

$$w_c = \frac{\operatorname{median}(f)}{f_c + \epsilon}$$

实现方式为：class_weights = median / (freq + 1e-6)。因此，稀有类别会获得更大的权重。稀有类别会得到较高的权重，而占主导地位的类别会得到小得多的权重。这显著提升了针对细薄或罕见结构的分割质量。

- 4. **数据增强：** 遥感数据集的大小通常有限。数据增强可人为地增加训练多样性。该流程使用 Albumentations（HorizontalFlip、VerticalFlip、RandomRotate90、RandomBrightnessContrast），这些增强方法可提升对方向、光照、季节变化和传感器噪声的不变性。数学上，数据增强从 T ~ P(T) 中采样变换，并生成 Ĩ = T(I)，从而近似一个更大的训练分布。

分割模型通过 `smp.MAnet()` 实例化。该架构结合了：ResNet34 编码器、U-Net 解码器和注意力聚合模块。下面逐一详述。

编码器提取层次化特征图：F₁, F₂, ..., Fₗ，其中较深层包含越来越语义化的表示。预训练的 ResNet34 编码器最初在 ImageNet 数据集上训练，因此可以利用自然图像表示进行迁移学习。

解码器逐步对潜在特征进行上采样：

```
F_{l-1} = U(F_l),
```

其中 U 表示上采样。

跳跃连接融合编码器和解码器的表示：

```
ĤF_i = [F_i^{enc}, F_i^{dec}]
```

从而保留精细的空间细节。

混合注意力模块自适应地对重要的空间区域和通道进行加权。注意力系数通过以下方式学习：

```
A = σ(WF + b),
```

其中 F 表示特征图，W 是可学习参数，σ 是 sigmoid 函数。

精炼后的特征表示为：

```
F' = A ⊙ F
```

其中 ⊙ 表示逐元素相乘。这使网络能够聚焦于道路、水体边界和建筑物等语义重要的区域。

Dice 系数衡量预测与真实标签之间的重叠程度。

对于预测（P）和真实标签（G），Dice(P, G) = 2|P∩G| / |P|+|G|。

因此，Dice 损失为 L_Dice = 1 − Dice(P, G)。Dice 损失对不平衡的分割问题尤其有效，因为它直接优化区域重叠。

训练目标结合了加权交叉熵和 Dice 损失。数学上，

$$\mathcal{L} = \lambda \mathcal{L}_{CE} + (1-\lambda)\mathcal{L}_{Dice},$$

其中 $\lambda = 0.5$。这种混合形式结合了：

- 像素级分类准确率
- 区域级重叠优化

网络使用 **AdamW** 优化，它将权重衰减与自适应梯度估计解耦。此外，还通过函数 `clip_grad_norm_()` 进行梯度裁剪。这会强制满足 $\|\nabla\theta\|_2 \leq 1$，从而防止梯度爆炸，使优化过程更稳定。

当优化停滞时，调度器通过函数 `torch.optim.lr_scheduler.ReduceLROnPlateau` 降低学习率。如果验证损失进入平台期，学习率将变为：

$$\eta_{t+1} = 0.5\eta_t$$

自适应调度可改善局部最小值附近的收敛。

这里，训练损失稳步下降。这表明收敛成功。基于 Dice 的损失下降意味着预测区域与真实标签区域之间的重叠在不断增加。

训练好的模型使用函数 `torch.save()` 保存。这会存储模型权重、优化器状态和训练轮次，从而可以在之后恢复训练，而不会丢失优化器的统计信息。

由于 GPU 内存限制，大型卫星影像无法直接处理。解决方案是分块推理。先对影像进行填充，然后独立分割每个图块：

$$\hat{S}_{ij} = f_{\theta}(P_{ij})$$

随后将预测出的图块拼接在一起，最后移除填充。这样就重建出整幅卫星影像的分割图。现在，请参考下面的代码片段：

```
```python
# run on GPU / in colab
# CORE SETUP
path = kagglehub.dataset_download(
    "hammadjavaid/multi-source-satellite-imagery-for-segmentation"
)
PATCH = 512

def extract_patches(img, mask):
    H, W, _ = img.shape
    patches = []
    for y in range(0, H - PATCH, PATCH):
        for x in range(0, W - PATCH, PATCH):
            im = img[y:y + PATCH, x:x + PATCH]
```
```

```
```python
m = mask[y:y + PATCH, x:x + PATCH]
patches.append((im, m))
return patches

model = smp.MAnet(
    encoder_name="resnet34", encoder_weights="imagenet",
    classes=NUM_CLASSES, activation=None
).to(device)

optimizer = torch.optim.AdamW(model.parameters(), lr=1e-3)
scheduler = torch.optim.lr_scheduler.ReduceLROnPlateau(optimizer, factor=0.5)

# CLASS WEIGHTS (imbalanced learning)
class_weights = torch.tensor(
    class_weights, dtype=torch.float32
).to(device)

def dice_loss(pred, gt):
    # LOSS FUNCTION (Dice + CE)
    pred = torch.softmax(pred, dim=1)
    gt_onehot = F.one_hot(gt, NUM_CLASSES).permute(
        0, 3, 1, 2
    ).float()
    inter = (pred * gt_onehot).sum((2, 3))
    union = pred.sum((2, 3)) + gt_onehot.sum((2, 3))
    dice = (2 * inter + 1e-6) / (union + 1e-6)
    return 1 - dice.mean()

# DATA LOADER (patch dataset assumed pre-built)
loader = DataLoader(ds, batch_size=8, shuffle=True)

for epoch in range(10):  # TRAINING LOOP
    model.train()
    total_loss = 0

    for x, y in loader:
        x, y = x.to(device), y.to(device)
        optimizer.zero_grad()
        pred = model(x)
        loss = (
            0.5 * F.cross_entropy(
```
```

```
pred, y, weight=class_weights
) + 0.5 * dice_loss(pred, y)
)
loss.backward()
torch.nn.utils.clip_grad_norm_(model.parameters(), 1.0)
optimizer.step()
total_loss += loss.item()

avg_loss = total_loss / len(loader)
scheduler.step(avg_loss)
print(f"Epoch {epoch}: Loss {avg_loss:.4f}")

# Epoch 6 Loss 0.6810934002238108
# Epoch 7 Loss 0.6643141189464052
# Epoch 8 Loss 0.6605846921304115
# Epoch 9 Loss 0.637118750452099

# SAVE MODEL
torch.save(
    {"model": model.state_dict(),
    "optimizer": optimizer.state_dict(),
    "epoch": epoch},
    "checkpoint_MAUnet.pth"
)

# inference
model.eval()
img, gt = ds[0]
img = img / img.max()

with torch.no_grad():
    pred = model(img.unsqueeze(0).to(device))
pred = torch.argmax(pred, 1).squeeze().cpu().numpy()

# plot img, ground truth, and pred side-by-side here
```

如果运行上述代码并将图像并排显示，你应该会得到类似下图的结果：
