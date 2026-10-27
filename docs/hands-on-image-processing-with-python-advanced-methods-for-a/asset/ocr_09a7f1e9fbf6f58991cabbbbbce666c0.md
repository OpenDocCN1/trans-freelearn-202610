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

> **Note**
> 
> Channel attention is typically applied first to emphasize the most informative feature maps, providing a stronger and less noisy representation on which spatial attention can subsequently localize important regions more effectively.

**Adding Attention to a CNN:** The *attention* modules are inserted after *convolutional* and *batchnorm* blocks.

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

> **Note**
> 
> Batch Normalization (**BatchNorm**) layer normalizes intermediate activations to have approximately zero mean and unit variance within a mini-batch, stabilizing and accelerating training while improving convergence.