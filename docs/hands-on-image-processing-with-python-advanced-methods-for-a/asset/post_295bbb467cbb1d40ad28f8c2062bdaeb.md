
| Method | Accuracy |
|---|---|
| Supervised (10% labels) | 89.4% |
| Semi-Supervised (10% + unlabeled) | 92.8% |

*Table 12.6: Supervised vs. Semi-supervised classification accuracy*

Despite using exactly the same number of labeled samples, SSL achieves higher accuracy by exploiting the unlabeled data.

**Visualization:** The code produces the following couple of visualizations:

## Prediction Grid

```
preds_sup = model_sup(X_test_t[:10]).argmax(1)
preds_ssl = model_ssl(X_test_t[:10]).argmax(1)
```

The first row shows predictions from the supervised model, while the second row shows predictions from the SSL model. Green labels indicate correct classifications and red labels indicate errors.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_631_0.png)

*Figure 12.19 – Semi-supervised classification of digit images*

## Accuracy Comparison

```
plt.bar(labels, accuracies)
```

The following bar chart compares the performance of supervised and semi-supervised learning, clearly illustrating the benefit of leveraging unlabeled data.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_631_1.png)

*Figure 12.20 – Supervised vs. Semi-supervised classification accuracy with digit images*

Traditional supervised learning learns only from labeled images: Few Labels → Classifier. Semi-supervised learning additionally learns from the structure of unlabeled images:

Few Labels + Many Unlabeled Images → Better Classifier.

The unlabeled data acts as a form of guidance, helping the network discover more accurate decision boundaries. As the model improves, its pseudo-labels become more reliable, allowing it to progressively learn from a larger portion of the unlabeled dataset. This ability to exploit abundant unlabeled data is what makes semi-supervised learning particularly attractive for modern image-classification applications.

## Masked Autoencoders (MAE): Self-Supervised learning by reconstructing missing image patches

Masked Autoencoders (MAE) are a powerful self-supervised learning framework that learns visual representations without requiring image labels. Instead of predicting class labels, the model learns by reconstructing missing image regions from the visible portions of the image.

The central idea is remarkably simple:

- Split an image into patches
- Randomly mask a large fraction of patches (typically 75%)
- Feed only the visible patches into a Vision Transformer (ViT) encoder
- Train a lightweight decoder to reconstruct the missing patches

By solving this reconstruction task, the network learns rich semantic representations that can later be transferred to image classification, object detection, segmentation, and other vision tasks. MAE learns similarly to solving a jigsaw puzzle. Even when most pieces are missing, the model learns to infer the missing content from the surrounding context.

> Note
> 
> Self-Supervised Learning (SSL) learns useful feature representations from unlabeled data by automatically generating supervision from the data itself rather than using human-provided labels. Masked Autoencoders (MAE) are a form of SSL because they randomly mask a large fraction of image patches and train a Vision Transformer to reconstruct the missing patches from the visible ones, using the original image as its own supervision signal.

MAE Architecture: A Masked Autoencoder consists of:

- Patch Embedding Layer
- Vision Transformer Encoder
- Masking Module
- Lightweight Decoder
- Reconstruction Loss

The model is pretrained by minimizing the reconstruction error between the original and reconstructed image patches. The reconstruction objective is:

$$L = \frac{1}{N}\sum_{i=1}^{N}|x_i - \hat{x}_i|^2$$

where $x_i$ = original patch, $\hat{x}_i$ = reconstructed patch, $N$ = number of masked patches. Unlike supervised learning, no class labels are required.

**Loading a Pretrained MAE:** The following code loads a pretrained MAE model from Hugging Face:

```
processor = AutoImageProcessor.from_pretrained("facebook/vit-mae-base")
model = ViTMAEForPreTraining.from_pretrained("facebook/vit-mae-base")
```

The model has already been pretrained on millions of images using the MAE reconstruction objective.

**Dataset Preparation:** For demonstration, a CIFAR-10 image is resized to the ViT input size.

```
dataset = CIFAR10(root="./data", train=False, download=True,
                 transform=transforms.Resize((224,224)))
img, label = dataset[0]
```

The image serves as input for reconstruction and feature extraction experiments.

**Reconstruction of Missing Patches:** After preprocessing, the image is passed through the pretrained MAE.

```
inputs = processor(images=img, return_tensors="pt")
with torch.no_grad():
    outputs = model(**inputs)
```

The model returns:

```
loss, reconstruction = outputs.loss, outputs.logits
```

where loss = reconstruction error and logits = reconstructed image patches. The reconstruction loss indicates how accurately the model can recover missing information.

```
print(loss.item())
```

A low reconstruction loss implies that the model has learned meaningful visual structure and context.

**Visualizing MAE Reconstruction:** The reconstructed patches are rearranged into the original image layout.

```
reconstruction = reconstruction.squeeze()
reconstruction = reconstruction.reshape(num_patches, num_patches, \
                                        patch_size, patch_size, 3)
```

The resulting visualization compares the original image and the reconstructed image.

```
ax[0].imshow(img_np); ax[1].imshow(reconstruction)
```

Even though large portions of the image were hidden during pretraining, the model can often recover the overall object structure surprisingly well.

**Random Patch Masking:** MAE typically masks about 75% of image patches during training. The following visualization illustrates the masking process:

```
for y in range(0,224,16):
    for x in range(0,224,16):
        if np.random.rand() < 0.75:
            masked[y:y+16, x:x+16] = 0
```

The masked image contains only a small subset of visible patches. By hiding most of the image, the model is forced to learn high-level semantic concepts rather than memorizing pixels.

**Vision Transformer Patch Representation:** MAE inherits the patch-based representation used by Vision Transformers. For a patch_size of 16, a 224 × 224 image becomes 14 × 14 = 196 patch tokens. The following visualization shows the patch grid:

```
ax.axhline(i); ax.axvline(i)
```

Each patch becomes an individual token processed by the transformer.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_634_0.png)

*Figure 12.21 – Image reconstruction with MAE*

**Patch Activation Map:** The encoder generates a feature vector for every image patch.

```
tokens = model.vit(inputs["pixel_values"]).last_hidden_state
```

After removing the CLS token, the feature magnitude of each patch is computed:

```
patch_tokens = tokens[0,1:,:]
activation_map = torch.norm(patch_tokens, dim=1)
```

This produces a heatmap indicating which image regions generate the strongest transformer activations. Bright regions correspond to patches that contain more informative visual content.

The resulting embedding summarizes the visual content of the image. These learned representations are often highly transferable to downstream tasks.

**PCA Visualization of MAE Features:** Features extracted from 500 CIFAR-10 images are projected into two dimensions using PCA.

```
pca = PCA(n_components=2)
emb = pca.fit_transform(features)
```

The scatter plot reveals how MAE organizes images in feature space. Images belonging to similar semantic categories often form visible clusters despite the model never having seen class labels during pretraining.

**t-SNE Visualization of Learned Representations:** A more powerful nonlinear visualization is obtained using t-SNE.

```
tsne = TSNE(n_components=2, perplexity=30)
emb = tsne.fit_transform(features)
```

The resulting feature map often exhibits clear semantic grouping of objects. This demonstrates that MAE learns meaningful representations even though its training objective is purely reconstruction-based.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_636_0.png)

*Figure 12.24 – MAE feature space in 2D manifold with PCA and t-SNE*

**Why MAE works:** Traditional supervised learning optimizes: Image → Class Label, MAE instead optimizes: Visible Patches → Missing Patches. Because reconstruction requires understanding object structure, texture, shape, and context, the model naturally learns rich visual representations. Unlike supervised learning, millions of unlabeled images can be utilized. The key insight is that **reconstructing missing image patches forces the model to learn semantic understanding of visual content**, making MAE an effective foundation model for modern computer vision tasks.
