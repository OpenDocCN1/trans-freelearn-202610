

return next_layer, skip_connection

def upsampling_block(expansive_input, contractive_input, n_filters=32):
    up = Conv2DTranspose(n_filters, kernel_size = 3, strides=(2,2),
                         padding='same')(expansive_input)
    merge = concatenate([up, contractive_input], axis=3)
    conv = Conv2D(n_filters, kernel_size = 3, activation='relu',
                  padding='same',
                  kernel_initializer=tf.keras.initializers.HeNormal())(merge)
    conv = Conv2D(n_filters, kernel_size = 3, activation='relu',
                  padding='same',
                  kernel_initializer=tf.keras.initializers.HeNormal())(conv)
    return conv

def unet_model(input_size=(96, 128, 3), n_filters=32, n_classes=23):
    inputs = Input(input_size)
    #contracting path
    cblock1 = conv_block(inputs, n_filters)
    cblock2 = conv_block(cblock1[0], 2*n_filters)
    cblock3 = conv_block(cblock2[0], 4*n_filters)
    cblock4 = conv_block(cblock3[0], 8*n_filters, dropout_prob=0.3)
    cblock5 = conv_block(cblock4[0],16*n_filters, dropout_prob=0.3, \
                         max_pooling=None)
    # expanding path
    ublock6 = upsampling_block(cblock5[0], cblock4[1], 8 * n_filters)
    ublock7 = upsampling_block(ublock6, cblock3[1], n_filters*4)
    ublock8 = upsampling_block(ublock7,cblock2[1] , n_filters*2)
    ublock9 = upsampling_block(ublock8,cblock1[1], n_filters)
    conv9 = Conv2D(n_filters, 3, activation='relu', padding='same',
                   kernel_initializer='he_normal')(ublock9)
    conv10 = Conv2D(n_classes, kernel_size=1, padding='same')(conv9)
    model = tf.keras.Model(inputs=inputs, outputs=conv10)
    return model

unet = unet_model((img_height, img_width, num_channels))

@tf.keras.utils.register_keras_serializable()
class MeanIoUWrapper(tf.keras.metrics.MeanIoU):
    def __init__(self, num_classes, name="mean_iou", **kwargs):
        super().__init__(num_classes=num_classes, name=name, **kwargs)
    def update_state(self, y_true, y_pred, sample_weight=None):
        y_pred = tf.argmax(y_pred, axis=-1)
        y_true = tf.squeeze(tf.cast(y_true, tf.int32), axis=-1)
        return super().update_state(y_true, y_pred, sample_weight)

miou_metric = MeanIoUWrapper(NUM_CLASSES)
unet.compile(optimizer='adam',
    loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True),
    metrics=['accuracy', miou_metric])
model_history = unet.fit(train_dataset, epochs=EPOCHS)
# Epoch 1/25
# accuracy: 0.3421 - loss: 2.2086 - mean_iou: 0.0528
# ...
# Epoch 25/25
# accuracy: 0.9659 - loss: 0.1007 - mean_iou: 0.7020

plt.plot(model_history.history["accuracy"])
plt.plot(model_history.history["loss"])

以下图表显示了训练 epoch 期间的准确率和损失曲线：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_570_0.png)

*图 11.11 – 训练 epoch 期间准确率的提升*

现在，损失的下降：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_570_1.png)

*图 11.12 – 训练 epoch 期间损失的下降*

使用模型对测试输入图像进行推理并并排绘制原始图像、真实和预测掩码将产生如下图所示的图像：

## 使用训练 30 个 epoch 的 U-Net 进行语义分割

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_571_0.png)

图 11.13 – 使用在 Lyft-Udacity 数据集上训练的 U-Net 模型的语义分割

评估循环在测试数据集上计算平均 IoU。最终性能：平均 IOU ≈ 0.69，这对于在调整大小的图像上训练的紧凑型 U-Net 来说是一个很强的性能。现在，请参考以下代码片段：

```
def evaluate_model(model, dataset): # model evaluation
    metric = MeanIoUWrapper(NUM_CLASSES)
    for images, masks in dataset:
        logits = model(images, training=False)
        metric.update_state(masks, logits)
    print("\nMean IoU:", metric.result().numpy())
    return metric

processed_test_image_ds.batch(1)
test_dataset = processed_test_image_ds.cache().shuffle(BUFFER_SIZE).batch(1)
evaluate_model(unet, test_dataset)
# Mean IoU: 0.69191945
```

## 使用 segmentation_models_pytorch 的基于 ResNet18 主干的 U-Net

现代分割系统通常使用预训练编码器来提高收敛速度、特征质量和泛化能力。不是从头训练编码器，而是重用预训练的 CNN 主干。这种方法被称为编码器-解码器迁移学习。

下面的代码片段演示了使用 segmentation_models_pytorch 实现的预训练 ResNet18 编码器的 U-Net 模型的语义分割，segmentation_models_pytorch 提供了生产就绪的架构，如 U-Net、FPN、PSPNet 和 DeepLabV3+，具有可互换的主干。

自定义 PyTorch Dataset 类使用 Albumentations 加载和预处理 Lyft-Udacity 道路场景图像和掩码，而 DataLoader 实现了高效的批处理和 GPU 训练。模型 smp.Unet(encoder_name="resnet18") 使用预训练的 ResNet18 编码器进行迁移学习，其中残差连接改善了梯度流动和特征提取。解码器从分层语义特征重建密集分割掩码。

训练使用 nn.CrossEntropyLoss() 进行前向传播、反向传播和优化器更新，而分割质量使用像素准确率、平均 IoU 和 Dice 系数进行评估。预测使用 torch.argmax() 转换为语义标签，并使用 Cityscapes 风格的颜色映射进行可视化。与自定义 TensorFlow U-Net 相比，SMP 实现更具可扩展性、训练速度更快，并且由于预训练编码器和工业级抽象，更适合实际部署。现在，请参考代码片段：

```
from torch.utils.data import Dataset, DataLoader
import segmentation_models_pytorch as smp
from torchmetrics.classification import (MulticlassAccuracy,
                                        MulticlassJaccardIndex,
                                        MulticlassF1Score)

train_images = sorted(glob.glob("lyft-udacity-challenge/dataA/dataA/CameraRGB/*.png"))
train_masks  = sorted(glob.glob("lyft-udacity-challenge/dataA/dataA/CameraSeg/*.png"))

class LyftDataset(Dataset):  # DATASET
    def __len__(self):
        return len(self.images)

    def __getitem__(self, idx):
        image = imageio.imread(self.images[idx])
        mask = imageio.imread(self.masks[idx])
        mask = np.max(mask, axis=-1)
        augmented = self.transform(image=image, mask=mask)
        image = augmented["image"]
        mask = augmented["mask"].long()
        return image, mask

train_loader = DataLoader(LyftDataset(train_images, train_masks),
    batch_size=BATCH_SIZE, shuffle=True, num_workers=2, pin_memory=True)
test_loader = DataLoader(LyftDataset(test_images, test_masks),
    batch_size=BATCH_SIZE, shuffle=False, num_workers=2, pin_memory=True)
model = smp.Unet(encoder_name="resnet18", encoder_weights=None,
                in_channels=3, classes=NUM_CLASSES).to(DEVICE)
criterion = nn.CrossEntropyLoss()
optimizer = torch.optim.Adam(model.parameters(), lr=LR)
train_miou_metric = MulticlassJaccardIndex(num_classes=NUM_CLASSES,
average="macro").to(DEVICE)

for epoch in range(EPOCHS):  # TRAINING LOOP
```

```
model.train()
epoch_loss = 0.0
train_miou_metric.reset()
for images, masks in tqdm(train_loader, desc=f"Epoch {epoch+1}/{EPOCHS}"):
    images = images.to(DEVICE)
    masks = masks.to(DEVICE)
    optimizer.zero_grad()
    outputs = model(images)
    loss = criterion(outputs, masks)
    loss.backward()
    optimizer.step()
    epoch_loss += loss.item()
    preds = torch.argmax(outputs, dim=1)
    train_miou_metric.update(preds, masks)

torch.save(model.state_dict(), "unet_smp.pth")
model.eval()

with torch.no_grad():
    for images, masks in tqdm(test_loader, desc="Evaluating"):
        images = images.to(DEVICE)
        masks = masks.to(DEVICE)
        outputs = model(images)
        preds = torch.argmax(outputs, dim=1)

model.eval()
with torch.no_grad():
    images, masks = next(iter(test_loader))
    outputs = model(images)
    preds = torch.argmax(outputs, dim=1)

image_np = images[0].cpu().permute(1,2,0).numpy()
true_mask = masks[0].cpu().numpy()
pred_mask = preds[0].cpu().numpy()
# Plot image_np, true_mask and pred_mask side-by-side here

# Epoch [25/25]
# Loss      : 0.1301
# Accuracy  : 0.7326
# Mean IoU  : 0.6693
# Mean Dice : 0.7567
```

