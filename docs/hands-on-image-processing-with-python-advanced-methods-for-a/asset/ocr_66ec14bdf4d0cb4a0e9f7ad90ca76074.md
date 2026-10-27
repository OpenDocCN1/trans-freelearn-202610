implemented as: class_weights = median / (freq + 1e-6). Rare classes, therefore, receive larger weights. A rare class receives a higher weight, while dominant classes receive much smaller weights. This significantly improves segmentation quality for thin or infrequent structures.

+   4. **Data augmentation:** Remote sensing datasets are typically limited in size. Data augmentation artificially increases training diversity. The pipeline uses Albumentations (HorizontalFlip, VerticalFlip, RandomRotate90, RandomBrightnessContrast), these augmentations improve invariance to orientation, illumination, seasonal variations and sensor noise. Mathematically, augmentation samples transformations T ~ P(T) and generates Ĩ = T(I) thus, approximating a larger training distribution.

The segmentation model is instantiated with `smp.MAnet()`. The architecture combines: ResNet34 encoder, U-Net decoder, and Attention aggregation modules. Let's consider them in detail.

The encoder extracts hierarchical feature maps: F₁, F₂, ..., Fₗ, where deeper layers contain increasingly semantic representations. The pretrained ResNet34 encoder was originally trained on the ImageNet dataset, enabling transfer learning from natural-image representations.

The decoder progressively upsamples the latent features:

```
F_{l-1} = U(F_l),
```

where U denotes upsampling.

Skip connections fuse encoder and decoder representations:

```
ĤF_i = [F_i^{enc}, F_i^{dec}]
```

preserving fine spatial details.

Mixed-attention modules adaptively weigh important spatial regions and channels. Attention coefficients are learned as:

```
A = σ(WF + b),
```

where F denotes feature maps, W are learnable parameters and σ is the sigmoid function.

The refined feature representation becomes:

```
F' = A ⊙ F
```

where ⊙ denotes elementwise multiplication. This allows the network to focus on semantically important regions such as roads, water boundaries, and buildings.

The Dice coefficient measures overlap between prediction and ground truth.

For prediction (P) and ground truth (G), Dice(P, G) = 2|P∩G| / |P|+|G|.

The Dice loss is therefore L_Dice = 1 − Dice(P, G). Dice loss is especially effective for imbalanced segmentation problems because it directly optimizes region overlap.