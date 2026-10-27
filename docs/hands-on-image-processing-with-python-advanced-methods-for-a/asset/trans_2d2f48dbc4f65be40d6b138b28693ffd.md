
```text
"Golden retriever" → dog
"Beagle" → dog
```

因此，模型预测可以写作：

$\hat{y}=g(f_{\mathrm{ImageNet}}(x)),$

其中$g(\cdot)$表示类别合并操作。主要优点是无需额外训练，展示了ImageNet特征的可迁移性，并且部署非常简单：

```python
pretrained_vgg16_model = VGG16(weights='imagenet', include_top=True)
```

主要局限性在于：网络是针对1000个ImageNet类别而非猫狗分类优化的；当预测的品种不在映射中时会产生错误；分类边界也未针对目标数据集进行调整。

## 方法2：使用VGG16进行迁移学习

更有效的策略是复用卷积特征提取器，并训练一个新的分类头。原始VGG16分类器被移除。

```python
base_model = VGG16(weights='imagenet', include_top=False,
    input_shape=(224,224,3))
```

仅保留卷积主干网络。

**冻结特征提取器：** 预训练参数被冻结。

```python
for layer in base_model.layers:
    layer.trainable = False
```

从数学角度看，主干网络变为固定映射$\mathbf{z}=f_{\mathrm{VGG16}}(\mathbf{x})$，其中$\mathbf{z}\in\mathbb{R}^{7\times7\times512}$表示高层特征张量。训练期间仅优化新添加的分类层。

**设计新的分类头：** 输出特征张量通过全局平均池化转换为紧凑的特征向量：

$z_k=\frac{1}{HW}\sum_{i=1}^{H}\sum_{j=1}^{W}F_k(i,j),$

其中$F_k$表示第$k$个特征图。该操作大幅减少参数量并缓解过拟合。分类器头为：

```python
base_model = VGG16(weights='imagenet', include_top=False,
    input_shape=(224,224,3))
x = base_model.output
x = GlobalAveragePooling2D()(x)
x = Dense(256, activation='relu')(x)
x = Dropout(0.3)(x)
output = Dense(1, activation='sigmoid')(x)
```

最终预测为：$\hat{y}=\sigma(\mathbf{w}^{T}\mathbf{z}+b)$，其中$\sigma(t)=\frac{1}{1+e^{-t}}$是sigmoid激活函数。

### 二元交叉熵损失

由于任务仅包含两个类别，网络使用二元交叉熵进行训练：

$\mathcal{L} = -y \log(\hat{y}) - (1 - y)\log(1 - \hat{y}),$

其中$y \in \{0,1\}$为真实标签。

优化目标为：

$\theta^* = \arg\min_{\theta} \sum_{i=1}^{N} \mathcal{L}_i$

使用Adam优化器求解：

```python
transfer_learning_model.compile(optimizer=Adam(learning_rate=1e-4), # compile
loss='binary_crossentropy', metrics=['accuracy'])
```

### 模型训练

网络训练五个轮次（epochs）。

```python
transfer_learning_model.fit(train_data, validation_data=test_data, epochs=5)
# Epoch 5/5
# [1m250/250[0m [32m------------------------------[0m[37m[0m [1m68s[0m 271ms/step -
# accuracy: 0.9808 - loss: 0.0537 - val_accuracy: 0.9835 - val_loss: 0.0585
```

由于主干网络已经包含从ImageNet学习到的高判别性视觉表示，训练快速收敛。下图展示了观察到的学习曲线。在整个训练过程中，验证损失稳步下降，表明预训练特征成功适应了新任务。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_608_0.png)

图12.4 – 迁移学习训练与损失

## 性能评估

在两个模型上，使用未见过的测试数据集评估分类准确率：

```python
def evaluate_pretrained_vgg16():
    predictions, targets = [], []
    for images, labels in test_data:
        outputs = pretrained_vgg16_model.predict(images, verbose=0)
        decoded = decode_predictions(outputs, top=1)[0][0][1]
        predictions.append(map_imagenet_class(decoded))
        targets.append(idx_to_class[int(labels[0])])
    print("Pretrained VGG16 Accuracy:", accuracy_score(targets, predictions))
    return acc

pretrained_acc, _, _ = evaluate_pretrained_vgg16() # evaluate pretrained model
transfer_acc, _, _ = evaluate_transfer_vgg16() # evaluate TL model (refer GitHub)
```

**预训练的VGG16：** 该ImageNet模型取得了96.22%的测试准确率，混淆矩阵如下：

[668  47]
[  1  554]

测试数据集上的分类报告如下：

| 指标 | 猫 | 狗 |
|---|---|---|
| **精确率** | **1.00** | **0.92** |
| **召回率** | **0.93** | **1.00** |
| **F1分数** | **0.97** | **0.96** |

*表12.3：使用预训练模型在测试数据集上的分类报告*

考虑到该模型从未专门针对猫狗分类进行训练，这一结果出人意料地强大。

**迁移学习VGG16：** 在替换分类头并重新训练后，测试准确率提升至98.35%，混淆矩阵为[990  10; 23  977]。这一改进证明了将预训练特征适应到目标领域的有效性。

**分类指标：** 迁移学习模型取得：

| 指标 | 猫 | 狗 |
|---|---|---|
| 精确率 | 0.98 | 0.99 |
| 召回率 | 0.99 | 0.98 |
| F1分数 | 0.98 | 0.98 |

*表12.4：使用微调模型（带迁移学习）在测试数据集上的分类报告*

均衡的精确率和召回率表明模型在两个类别上都表现良好。预训练模型学习了一个为ImageNet优化的决策函数$f_{\mathrm{ImageNet}}: \mathcal{X} \to \mathbb{R}^{1000}$。迁移学习在保留已学到的视觉表示的同时，修改最终分类器：$f_{\text{cats-dogs}}: \mathcal{X} \to \mathbb{R}^2$。卷积滤波器继续检测耳朵、毛发纹理、眼睛和身体形状，但最终决策边界变得专门用于区分猫和狗。

**预测结果可视化：** 下图展示了测试数据集中预训练模型失败、但微调（FT，带迁移学习）预测正确分类的一些图像。迁移学习后的VGG16能产生更具任务针对性和鲁棒性的猫狗预测，尤其是在具有异常姿态、遮挡或外观变化的挑战性图像上。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_610_0.png)

*图12.5 – 微调VGG16预测成功而预训练模型失败的测试图像*

下图展示了数据集之外的猫和狗图像，预训练模型和微调VGG16模型均正确分类。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_610_1.png)

*图12.6 – 使用预训练模型与迁移学习模型预测数据集外图像的标签*

## 实践注意事项

**微调：** 在本实验中，所有卷积层均被冻结。一种常见的改进是部分微调：

```python
for layer in base_model.layers[-4:]:
    layer.trainable = True
```

允许最深层适应目标领域。

**数据增强：** 通常可以通过随机翻转、旋转、缩放和亮度扰动进一步提高性能，这些操作增加了训练多样性并减少了过拟合。

**现代替代方案：** 尽管VGG16在教学上仍然重要，但较新的架构通常以更少的参数提供更高的准确率：ResNet、EfficientNet、ConvNeXt、视觉Transformer（ViT）和Swin Transformer。本章稍后将探讨其中的许多模型。

这个例子展示了现代计算机视觉的核心思想之一：从大规模数据集中学习到的强大图像表示可以在许多下游任务中复用。预训练的VGG16网络尽管从未针对该目的训练，却已经在猫狗分类上取得了超过96%的准确率。通过将原始ImageNet分类器替换为轻量级任务特定头，并仅重新训练少量参数，迁移学习将准确率提升到98%以上。这种显著的性能提升，加上更短的训练时间和更少的数据需求，解释了为什么迁移学习已成为实际图像分类系统的默认策略。

## 使用VGG16进行迁移学习（PyTorch）

同样的迁移学习工作流也可以在PyTorch中实现，使用来自`torchvision.models`的预训练VGG16网络。卷积主干网络被冻结，最终分类器替换为二分类头，并且仅在新添加的层上使用Cats-vs-Dogs数据集进行训练。PyTorch实现用**ImageFolder**和**DataLoader**取代了Keras的**ImageDataGenerator**，使用`torchvision.models.VGG16`代替`tf.keras.applications.VGG16`，采用**BCEWithLogitsLoss**和**torch.optim.Adam**进行训练，并使用原生PyTorch API执行推理和模型保存。其性能与TensorFlow/Keras版本相当，将预训练模型的准确率从约**97.4%**提升到迁移学习后的**98.5%**。完整实现可在配套的GitHub仓库中获取，下图展示了模型对测试图像的分类结果。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_611_0.png)

虽然*迁移学习*复用了从大规模数据集中学到的知识，*注意力机制*通过使网络能够选择性地关注信息量最大的图像区域来进一步提升分类性能，我们将在下一节探讨这一点。

