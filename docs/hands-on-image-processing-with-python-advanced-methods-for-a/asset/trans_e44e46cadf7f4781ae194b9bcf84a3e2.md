

## 使用注意力网络的图像分类

传统的 CNN 对每个特征图和图像区域都同等重要地处理。然而，人类自然会关注场景中最具信息量的部分——例如猫的脸而不是背景的草地。注意力机制试图模仿这种行为，通过自动学习**要强调哪些特征以及在哪里查看**，从而提高分类准确性和可解释性。

### 带通道注意力和空间注意力的 CNN

考虑观察一张鸟的图像。某些特征图可能对羽毛、喙或翅膀做出强烈响应，而其他特征图可能包含很少有用的信息。通道注意力学习哪些特征图很重要，而空间注意力则识别与分类决策最相关的图像区域。

给定特征张量 $F \in \mathbb{R}^{H\times W\times C}$，通道注意力计算 $F' = M_c(F) \odot F$，而空间注意力进一步将特征细化为 $F'' = M_s(F') \odot F'$。

**通道注意力：** 直观上，通道注意力就像特征图的音量控制：有用的通道被放大，而信息较少的通道被衰减，如下一段代码所示。直观上，GlobalAveragePooling2D 询问“哪些特征在整个图像中持续存在？”，而 GlobalMaxPooling2D 询问“哪些特征在图像中的任何位置出现得最强？”，两者共同为计算通道重要性提供了互补的线索。

```
```python
class ChannelAttention(tf.keras.layers.Layer): # for complete code refer to GitHub

    def call(self, inputs):
        avg_pool = tf.keras.layers.GlobalAveragePooling2D()(inputs)
        avg_pool = self.shared_layer_one(avg_pool)
        avg_pool = self.shared_layer_two(avg_pool)
        max_pool = tf.keras.layers.GlobalMaxPooling2D()(inputs)
        max_pool = tf.keras.layers.Reshape((1,1,filters))(max_pool)
        max_pool = self.shared_layer_one(max_pool)
        max_pool = self.shared_layer_two(max_pool)
        attention = tf.keras.layers.Add()([avg_pool,max_pool])
        attention = tf.keras.layers.Activation('sigmoid')(attention)
        return tf.keras.layers.Multiply()([inputs, attention])
```
```

**空间注意力：** 空间注意力像聚光灯一样突出重要对象区域并抑制背景杂乱。通道注意力决定*什么*特征图（例如毛皮、眼睛、翅膀）很重要，而空间注意力决定图像中*哪里*这些重要特征位于。

```
```python
class SpatialAttention(tf.keras.layers.Layer):
    def call(self, inputs):
        avg_pool = tf.keras.layers.Lambda(
```

```
```python
lambda x: tf.keras.backend.mean(
    x, axis=3, keepdims=True)(inputs)
max_pool = tf.keras.layers.Lambda(
    lambda x: tf.keras.backend.max(
        x, axis=3, keepdims=True))(inputs)
attention = tf.keras.layers.Concatenate(
    axis=3)([avg_pool, max_pool])
attention = self.conv2d(attention)
return tf.keras.layers.multiply(
    [inputs, attention])
```
```

> **注意**
> 
> 通道注意力通常首先应用以强调最具信息量的特征图，提供更强且噪声更少的表示，以便空间注意力随后能更有效地定位重要区域。

**向 CNN 添加注意力：** *注意力*模块在 *卷积* 和 *批归一化* 块之后插入。

```
```python
import tensorflow as tf
print(tf.__version__)
# 2.18.0
simple_cnn_with_attention = tf.keras.model.Sequential([
    tf.keras.layers.Conv2D(32, 3, input_shape=ds_info.features['image'].shape,
                           activation='elu', padding='same'),
    tf.keras.layers.BatchNormalization(),
    tf.keras.layers.Conv2D(32, 3, padding='same', activation='elu'),
    tf.keras.layers.BatchNormalization(),
    ChannelAttention(32,8),
    SpatialAttention(7),
    ...                         # for complete code refer GitHub
])
```
```

> **注意**
> 
> 批归一化 (**BatchNorm**) 层将中间激活归一化为在 mini-batch 内近似零均值和单位方差，稳定并加速训练，同时提高收敛性。

函数 `tfds.load()` 自动下载（如需）、准备并加载 CIFAR-100 数据集作为 TensorFlow 数据管道，同时返回数据集元数据，如类别名称、图像形状和数据集大小。

```
import tensorflow_datasets as tfds
from tf_explain.core.grad_cam import GradCAM
gpus = tf.config.experimental.list_physical_devices('GPU')
(ds_train, ds_test), ds_info = tfds.load('cifar100', split=['train', 'test'],
                                      shuffle_files=True, as_supervised=True, with_
info=True)
```

网络在 CIFAR-100 上训练，并与一个相同的 *基线* CNN 模型 **不带注意力** 进行比较。下图显示了改进的学习行为。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_614_0.png)

*图 12.8 – 带和不带注意力的 CNN 的训练曲线*

模型首先预测图像类别，然后 **Grad-CAM** 从预测类别向最后的卷积层反向传播梯度，以生成类激活热图，显示哪些图像区域对决策贡献最大。比较 *基线* 和 *注意力增强* CNN 的热图提供了 **可解释 AI (XAI)** 可视化，有助于解释 *为什么* 会做出预测，以及模型是关注目标对象而不是无关背景区域，如下一段代码所示。

```
y_pred = model.predict(img[None])  # Prediction
pred_class = np.argmax(y_pred)
explainer = GradCAM()  # Grad-CAM explanation
heatmap = explainer.explain([img], None), model, class_index=pred_class,
                              image_weight=0.8)
plt.imshow(heatmap)  # Visualization
```

下图表明，注意力使 *Grad-CAM* 激活在目标对象周围更加集中。

## 使用带和不带注意力层的 ConvNets 进行图像分类的 GradCAM 可视化

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_615_0.png)

图 12.9 – 使用 CNN（带和不带注意力层）进行图像分类的 GradCAM 可视化

# 注意

tf.config.experimental 命名空间包含在 TensorFlow 急切执行转换期间引入的低级运行时控制，提供高级硬件和内存管理功能，这些功能最初标记为实验性，后来成为稳定 API 的一部分。

## 使用视觉 Transformer (ViT) 进行图像分类

CNN 使用局部感受野分析图像。视觉 Transformer 反而将图像视为一系列小块，类似于句子中的单词。自注意力允许每个块直接与其他每个块交互，从而使模型能够捕捉长程关系。对于 N 个图像块，

Attention(Q, K, V) = softmax( (Q K^T) / sqrt(d) ) V

在这些示例中，我们将使用 transformers 库（由 Hugging Face 开发），该库提供了轻松访问用于视觉、语言和多模态任务的最先进预训练 Transformer 模型。它让我们只需几行代码即可下载、预处理并使用预训练的 Transformer-based 视觉模型进行推理，而无需从头实现架构。预训练的 ViT 模型可以加载为：

```
from transformers import ViTImageProcessor, ViTForImageClassification
processor = ViTImageProcessor.from_pretrained("google/vit-base-patch16-224")
model = ViTForImageClassification.from_pretrained("google/vit-base-patch16-224", output_attentions=True)
```

使用预训练的 ViT 模型进行推理模式（`model.eval()`），输入图像被转换为块并处理，以获得类别预测（分类分数：logits）和自注意力图（attentions），随后可以可视化以揭示对模型决策影响最大的图像区域。

```
```python
model.eval()
inputs = processor(images=image, return_tensors="pt")  # Forward pass
with torch.no_grad():
    outputs = model(**inputs, output_attentions=True)
logits = outputs.logits
attentions = outputs.attentions
```
```

Attention rollout（参考 GitHub 中的函数 `rollout_attention()`）结合多个 Transformer 层的注意力图，以可视化哪些图像块对最终预测贡献最强。下图显示了叠加在输入图像上的典型注意力热图。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_616_0.png)

*图 12.10 – 可解释 AI – ViT 图像分类的注意力热图*

## 使用 ViT 进行花卉分类

微调预训练的 ViT（在牛津 102 花卉数据集上微调）可实现高度准确的花卉物种识别。与仅返回标签的传统分类器不同，Transformer 注意力图揭示了哪些花瓣、纹理和花卉结构影响了预测。

```
```python
model = AutoModelForImageClassification.from_pretrained(
    "MaulikkMadhavi/vit-base-flowers102", output_attentions=True)
```
```

上传的花卉图像使用预训练的 ViT 进行分类，显示前 5 个预测结果，以及注意力 rollout 热图，该热图突出显示对预测最负责任的图像区域，如以下代码片段所示：

```
```python
with torch.no_grad():
    outputs = model(**inputs, output_attentions=True)  # ViT inference
top5 = torch.topk(torch.nn.functional.softmax(outputs.logits, dim=-1)[0], k=5)
mask = rollout_attention(outputs.attentions)  # Attention Heatmap
```
```

