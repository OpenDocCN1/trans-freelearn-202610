
```text
"Golden retriever" → dog
"Beagle" → dog
```

The model prediction can therefore be written as:

$\hat{y}=g(f_{\mathrm{ImageNet}}(x)),$

where $g(\cdot)$ denotes the class-merging operation. The key advantages are that no additional training is required, it demonstrates the transferability of ImageNet features and it’s extremely simple to deploy:

```python
pretrained_vgg16_model = VGG16(weights='imagenet', include_top=True)
```

The key limitations are the network is optimized for 1000 ImageNet classes rather than cats-versus-dogs, errors occur when the predicted breed is not included in the mapping and classification boundaries are not adapted to the target dataset.

## Approach 2: Transfer learning using VGG16

A more effective strategy is to reuse the convolutional feature extractor and train a new classification head. The original VGG16 classifier is removed.

```python
base_model = VGG16(weights='imagenet', include_top=False,
    input_shape=(224,224,3))
```

Only the convolutional backbone is retained.

**Freezing the Feature Extractor:** The pretrained parameters are frozen.

```python
for layer in base_model.layers:
    layer.trainable = False
```

Mathematically, the backbone becomes a fixed mapping $\mathbf{z}=f_{\mathrm{VGG16}}(\mathbf{x})$ where $\mathbf{z}\in\mathbb{R}^{7\times7\times512}$ represents a high-level feature tensor. Only the newly added classification layers are optimized during training.

**Designing a New Classification Head:** The output feature tensor is converted into a compact feature vector using global average pooling:

$z_k=\frac{1}{HW}\sum_{i=1}^{H}\sum_{j=1}^{W}F_k(i,j),$

where $F_k$ denotes the $k$-th feature map. This operation dramatically reduces parameter count and mitigates overfitting. The classifier head is

```python
base_model = VGG16(weights='imagenet', include_top=False,
    input_shape=(224,224,3))
x = base_model.output
x = GlobalAveragePooling2D()(x)
x = Dense(256, activation='relu')(x)
x = Dropout(0.3)(x)
output = Dense(1, activation='sigmoid')(x)
```

The final prediction is: $\hat{y}=\sigma(\mathbf{w}^{T}\mathbf{z}+b)$, where $\sigma(t)=\frac{1}{1+e^{-t}}$ is the sigmoid activation function.

### Binary Cross-Entropy Loss

Because the task contains only two classes, the network is trained using binary cross-entropy:

$\mathcal{L} = -y \log(\hat{y}) - (1 - y)\log(1 - \hat{y}),$

where $y \in \{0,1\}$ is the ground-truth label.

The optimization objective is:

$\theta^* = \arg\min_{\theta} \sum_{i=1}^{N} \mathcal{L}_i$

which is solved using the Adam optimizer:

```python
transfer_learning_model.compile(optimizer=Adam(learning_rate=1e-4), # compile
loss='binary_crossentropy', metrics=['accuracy'])
```

### Model Training

The network is trained for five epochs.

```python
transfer_learning_model.fit(train_data, validation_data=test_data, epochs=5)
# Epoch 5/5
# [1m250/250[0m [32m------------------------------[0m[37m[0m [1m68s[0m 271ms/step -
# accuracy: 0.9808 - loss: 0.0537 - val_accuracy: 0.9835 - val_loss: 0.0585
```

Training rapidly converges because the backbone already contains highly discriminative visual representations learned from ImageNet. The observed learning curve is shown in the next figure. The validation loss decreases steadily throughout training, indicating successful adaptation of the pre-trained features to the new task.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_608_0.png)

Figure 12.4 – Transfer learning training vs. loss

## Performance evaluation

The classification accuracy is evaluated on the unseen test dataset for both models:

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

**Pretrained VGG16:** The ImageNet model achieves 96.22% test accuracy, with confusion matrix:

[668  47]
[  1  554]

with the following classification report on the test dataset:

| Metric | Cats | Dogs |
|---|---|---|
| **Precision** | **1.00** | **0.92** |
| **Recall** | **0.93** | **1.00** |
| **F1-score** | **0.97** | **0.96** |

*Table 12.3: Classification report on test dataset with pretrained model*

This result is surprisingly strong given that the model was never trained specifically for cats-versus-dogs classification.

**Transfer Learning VGG16:** After replacing the classification head and retraining, the test accuracy increases to 98.35%, with confusion matrix [990  10; 23  977]. The improvement demonstrates the effectiveness of adapting pretrained features to the target domain.

**Classification Metrics:** The transfer-learning model achieves:

| Metric | Cats | Dogs |
|---|---|---|
| Precision | 0.98 | 0.99 |
| Recall | 0.99 | 0.98 |
| F1-score | 0.98 | 0.98 |

*Table 12.4: Classification report on test dataset with fine-tuned model (with transfer learning)*

The balanced precision and recall values indicate that the model performs well on both categories. The pretrained model learns a decision function $f_{\mathrm{ImageNet}}: \mathcal{X} \to \mathbb{R}^{1000}$ optimized for ImageNet.

Transfer learning modifies the final classifier: $f_{\text{cats-dogs}}: \mathcal{X} \to \mathbb{R}^2$ while preserving the learned visual representation. The convolutional filters continue detecting ears, fur textures, eyes, and body shapes, but the final decision boundary becomes specialized for distinguishing cats from dogs.

**Visualizing Predictions:** The next plot shows a few images from the test dataset where the pretrained model fails, but the fine-tuned (FT, with transfer learning) predictions classify correctly. The transfer-learned VGG16 produces more task-specific and robust cat-versus-dog predictions, particularly for challenging images with unusual poses, occlusions, or appearance variations.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_610_0.png)

*Figure 12.5 – Test images where prediction with fine-tuned VGG16 succeeds but pretrained doesn’t*

The next figure shows a cat and a dog image from outside the dataset, where both the pretrained and fine-tuned VGG16 models classify correctly

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_610_1.png)

*Figure 12.6 – predicting labels of images outside the dataset with pretrained vs. transfer-learning models*

## Practical considerations

**Fine-Tuning:** In this experiment all convolutional layers were frozen. A common improvement is partial fine-tuning:

```python
for layer in base_model.layers[-4:]:
    layer.trainable = True
```

allowing the deepest layers to adapt to the target domain.

**Data Augmentation:** Performance can often be improved further using random flips, rotations, zooming, and brightness perturbations, which increase training diversity and reduce overfitting.

**Modern Alternatives:** Although VGG16 remains pedagogically important, newer architectures typically provide higher accuracy with fewer parameters: ResNet, EfficientNet, ConvNeXt, Vision Transformer (ViT) and Swin Transformer. Many of these models will be explored later in this chapter.

This example demonstrates one of the central ideas of modern computer vision: powerful image representations learned from large-scale datasets can be reused across many downstream tasks. A pretrained VGG16 network already achieves over 96% accuracy on cats-versus-dogs classification despite never being trained for that purpose. By replacing the original ImageNet classifier with a lightweight task-specific head and retraining only a small number of parameters, transfer learning improves accuracy to over 98%. This dramatic performance gain, coupled with reduced training time and data requirements, explains why transfer learning has become the default strategy for practical image classification systems.

## Transfer learning with VGG16 (PyTorch)

The same transfer-learning workflow can be implemented in PyTorch using a pretrained VGG16 network from `torchvision.models`. The convolutional backbone is frozen, the final classifier is replaced with a binary classification head, and only the newly added layers are trained on the Cats-vs-Dogs dataset. The PyTorch implementation replaces Keras **ImageDataGenerator** with **ImageFolder** and **DataLoader**, uses `torchvision.models.VGG16` instead of `tf.keras.applications.VGG16`, employs **BCEWithLogitsLoss** and **torch.optim.Adam** for training, and performs inference and model saving using native PyTorch APIs. It achieves performance comparable to the TensorFlow/Keras version, improving accuracy from approximately **97.4%** for the pretrained model to **98.5%** after transfer learning. The complete implementation is available in the companion GitHub repository, while the next figure shows classification of test images with the models.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_611_0.png)

While *transfer learning* reuses knowledge learned from large-scale datasets, *attention mechanisms* further improve classification by enabling networks to focus selectively on the most informative image regions, as we shall explore in the next section.
