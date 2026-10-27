**Computational Complexity:** $QK^T \in \mathbb{R}^{N\times N}$. Thus, time: $\mathcal{O}(N^2 d)$ and memory: $\mathcal{O}(N^2)$, so that high-resolution images $\rightarrow$ large $N$ $\rightarrow$ expensive computation. This motivates hierarchical transformers (**SegFormer**), efficient decoders (**DPT**), and query-based methods (**DETR**, **Mask2Former**).

**From Tokens to Dense Predictions: Decoder Heads for Segmentation:** After encoding:

$Z_L \in \mathbb{R}^{N\times D}$,

which is reshaped:

$Z_L \rightarrow \mathbb{R}^{H/P \times W/P \times D}$,

different architectures map tokens to pixel predictions differently (different segmentation models adopt different decoder strategies).

# Pure ViT dense prediction — SETR

Early work such as SEgmentation TRansformer (**SETR**) demonstrated that tokens can be directly mapped to segmentation: $\hat{Y} = \text{Upsample}(W Z_L)$, with no hierarchy and minimal decoder. **SETR** treats an image as a **sequence of patches** and performs **pixel-wise segmentation using a transformer encoder without convolutions**. It's a model with **Pure ViT + Linear Decoder:** Uses pure ViT encoder, reshapes tokens to spatial grid, applies *1x1 convolution* and *bilinear upsampling* to full resolution. This is equivalent to per-token classification followed by interpolation.

**Limitation:** Poor spatial detail, weak boundary localization and high computational cost. This motivates more advanced architectures.

# Multi-scale feature fusion with DPT

The **Dense Prediction Transformer (DPT)** is a *pure Vision Transformer (ViT)* architecture adapted for dense prediction tasks such as segmentation and depth estimation. Unlike standard ViT, DPT introduces **multi-scale feature fusion**, projecting tokens back to spatial grids. Here is the key insight: Global attention $\rightarrow$ captures **scene context**, and Multi-scale fusion $\rightarrow$ restores **fine spatial detail**. **DPT** extracts features from multiple layers:

$F = \text{Fusion}(F_1, F_2, F_3, F_4), \quad \hat{Y} = \text{Upsample}(F)$

where early layers $\rightarrow$ fine details, deep layers $\rightarrow$ semantics, and fusion $\rightarrow$ accurate segmentation.

The next code snippet loads a pretrained DPT model from Hugging Face, processes an input image, and performs **semantic segmentation** by predicting a class label for each pixel. The predicted logits are resized to the original image resolution, converted into a segmentation map, then colorized and blended with the original image to visualize the segmentation result side-by-side.

The method `DPTForSemanticSegmentation.from_pretrained()` uses a **ViT-Large backbone** and outputs logits:

$\mathbf{L} \in \mathbb{R}^{C \times H' \times W'}$