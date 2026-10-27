```
```text
"Golden retriever" → dog
"Beagle" → dog
```
```

The model prediction can therefore be written as:

$\hat{y}=g(f_{\mathrm{ImageNet}}(x)),$

where $g(\cdot)$ denotes the class-merging operation. The key advantages are that no additional training is required, it demonstrates the transferability of ImageNet features and it’s extremely simple to deploy:

```
```python
pretrained_vgg16_model = VGG16(weights='imagenet', include_top=True)
```
```

The key limitations are the network is optimized for 1000 ImageNet classes rather than cats-versus-dogs, errors occur when the predicted breed is not included in the mapping and classification boundaries are not adapted to the target dataset.

# Approach 2: Transfer learning using VGG16

A more effective strategy is to reuse the convolutional feature extractor and train a new classification head. The original VGG16 classifier is removed.

```
```python
base_model = VGG16(weights='imagenet', include_top=False,
    input_shape=(224,224,3))
```
```

Only the convolutional backbone is retained.

**Freezing the Feature Extractor:** The pretrained parameters are frozen.

```
```python
for layer in base_model.layers:
    layer.trainable = False
```
```

Mathematically, the backbone becomes a fixed mapping $\mathbf{z}=f_{\mathrm{VGG16}}(\mathbf{x})$ where $\mathbf{z}\in\mathbb{R}^{7\times7\times512}$ represents a high-level feature tensor. Only the newly added classification layers are optimized during training.

**Designing a New Classification Head:** The output feature tensor is converted into a compact feature vector using global average pooling:

$z_k=\frac{1}{HW}\sum_{i=1}^{H}\sum_{j=1}^{W}F_k(i,j),$

where $F_k$ denotes the $k$-th feature map. This operation dramatically reduces parameter count and mitigates overfitting. The classifier head is

```
```python
base_model = VGG16(weights='imagenet', include_top=False,
    input_shape=(224,224,3))
x = base_model.output
x = GlobalAveragePooling2D()(x)
x = Dense(256, activation='relu')(x)
x = Dropout(0.3)(x)
output = Dense(1, activation='sigmoid')(x)
```
```

The final prediction is: $\hat{y}=\sigma(\mathbf{w}^{T}\mathbf{z}+b)$, where $\sigma(t)=\frac{1}{1+e^{-t}}$ is the sigmoid activation function.