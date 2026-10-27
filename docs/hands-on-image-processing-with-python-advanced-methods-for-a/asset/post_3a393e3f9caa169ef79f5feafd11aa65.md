
```
```python
seg_map[np.logical_or(seg_map==3, seg_map==4)] = 2
seg_map[seg_map==12] = 11
acc = np.sum((seg_map[ground_truth!=-19]==ground_truth[ground_truth!=-19]))/\
    np.sum(ground_truth!=-19)
cm = confusion_matrix(ground_truth[ground_truth!=-19],
    seg_map[ground_truth!=-19], labels=np.array([0,1,2,5,6,7,8,9,11,13]))
intersection = np.diag(cm)
union = np.sum(cm, 0) + np.sum(cm, 1) - np.diag(cm)
return acc, intersection, union

acc, intersection, union = evaluate_single(seg, gt)
class_iou = np.round(intersection / union, 5)
# pixel accuracy: 0.98714
# mean class IoU: 0.53555
```
```

You can also use a pretrained *DeepLabV3* model (e.g., `deeplabv3_resnet50`) from the *Torchvision* repository via `torch.hub.load()`, and run inference to generate the pixel-wise semantic segmentation map representing the most probable class for each pixel. is left as an exercise to the reader (see the accompanying GitHub repository). The next figure shows the predicted segmentation masks and overlays generated:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_522_0.png)

*Figure 10.21 – Semantic segmentation with pretrained DeepLabV3 with a ResNet-50 backbone loaded via PyTorch hub*

## DeepLabV3+ with `Keras_hub`

This example follows the same DeepLabV3 semantic-segmentation pipeline discussed previously and is left as an exercise to the reader (refer to the accompanying GitHub repository). The next figure shows the predicted segmentation masks and overlay visualizations produced by the KerasHub DeepLabV3+ model:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_522_1.png)

*Figure 10.22 – An image, the (semantic) segmentation mask and overlay image with DeepLabV3+ from `keras_hub`*

```
```python
else: output = model(img_tensor)
prob = F.softmax(output, dim=1)
prob_maps.append(prob)
mask = torch.argmax(prob, dim=1)
hard_masks.append(mask)
# Soft Voting Ensemble
ensemble_prob = torch.mean(torch.stack(prob_maps), dim=0)
ensemble_mask = torch.argmax(ensemble_prob, dim=1).squeeze().cpu().numpy()
# Majority Voting
hard_stack = torch.stack(hard_masks)
majority_mask = torch.mode(hard_stack, dim=0)[0].squeeze().cpu().numpy()
# Plot ensemble and majority masks side-by-side here
```
```

If you run the above code, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_525_0.png)

*Figure 10.24 – Multi-model segmentation and ensemble learning with SMP and Torchvision*

The ensemble approaches discussed above consolidate predictions from multiple semantic segmentation models to improve pixel-wise class accuracy. However, they remain fundamentally limited to assigning a single class label per pixel and do not differentiate between separate object occurrences of the same category. This limitation motivates a shift toward instance-aware segmentation, where the goal is not only to classify pixels but also to distinguish and localize individual object instances within a scene.

## Instance segmentation

Following semantic segmentation—where each pixel is assigned a class label—instance segmentation goes a step further by distinguishing *individual object instances* within the same class. For example, while semantic segmentation labels all pixels belonging to “cat” as one class, instance segmentation

assigns separate masks to each individual cat. Formally, given an image $I \in \mathbb{R}^{H\times W\times 3}$, instance segmentation aims to predict a set:

$$y = \{(M_i, c_i, b_i)\}_{i=1}^{N},$$

where $M_i \in \{0,1\}^{H\times W}$: binary mask for instance $i$, $c_i \in \{1,\ldots,C\}$: class label and $b_i \in \mathbb{R}^4$: bounding box.

## Mask R-CNN: A two-stage instance segmentation framework

Instance segmentation matured significantly with the introduction of **Mask R-CNN** by Kaiming He et al. (2017). Mask R-CNN extends **Faster R-CNN** by adding a parallel branch for predicting segmentation masks.

Mask R-CNN consists of three major components:

- **Backbone network:** A deep CNN (e.g., ResNet-50 + FPN) extracts feature maps:
  
  $$F = \phi(I),$$
  
  where $\phi$ is the backbone function.

- **Region Proposal Network (RPN):** Generates candidate object regions (anchors):
  
  $$\mathcal{R} = \{r_i\}_{i=1}^{K}$$
  
  Each region has an objectness score and bounding box refinement.

- **ROI heads (multi-task learning):** For each proposed region, **Classification head:** predicts class $c_i$, **Bounding box regressor:** refines $b_i$ and **Mask head:** predicts binary mask $M_i$. Mask R-CNN optimizes a **multi-task loss**:
  
  $$\mathcal{L} = \mathcal{L}_{cls} + \mathcal{L}_{box} + \mathcal{L}_{mask}$$

The overall training objective of Mask R-CNN is formulated as a *multi-task loss* that jointly supervise classification, bounding box regression, and mask prediction, along with a key alignment mechanism for accurate region-wise feature extraction. The individual components are defined as follows:

- **Classification Loss:**
  
  $$\mathcal{L}_{cls} = -\log p_{c^*},$$
  
  where $c^*$ is the ground-truth class.

- **Bounding Box Loss:**
  
  $$\mathcal{L}_{box} = \sum_{i\in x,y,w,h} \operatorname{SmoothL1}(t_i - t_i^*),$$
  
  where **Smooth $L_1$** loss is a robust alternative to standard $L_2$ loss. It behaves like L2 loss (quadratic) when the error is small and L1 loss (linear) when the error is large.

- **Mask Loss:** Unlike classification, mask prediction is pixel-wise **binary classification**:
  
  $$\mathcal{L}_{mask} = \frac{1}{m^2}\sum_{u,v}[y_{uv}\log \hat{y}_{uv} + (1-y_{uv})\log(1-\hat{y}_{uv})].$$

where $y_{uv}$: ground truth mask, $\hat{y}_{uv}$: predicted mask and $m \times m$: mask resolution (typically 28×28).

- **ROIAlign:** Mask R-CNN replaces ROI Pooling with **ROIAlign**, eliminating quantization errors:

$$F_{aligned}(x, y) = \sum_{i,j} w_{ij}\,F(x_i, y_j)$$

This bilinear interpolation ensures **pixel-level alignment**, critical for accurate masks.

> **Note**
> 
> Bounding box regression is the process of learning to refine coarse object proposals into accurate object locations by predicting continuous coordinate offsets $(\Delta x, \Delta y, \Delta w, \Delta h)$ relative to an initial anchor or region proposal. Intuitively, instead of directly guessing the box, the model “nudges” a rough box so that it tightly fits the object, improving localization accuracy in object detection.

The following code snippet (refer to GitHub for complete code) demonstrates instance segmentation using Mask R-CNN with a ResNet-50 + Feature Pyramid Network (FPN) backbone via torchvision. `models.detection.maskrcnn_resnet50_fpn()`, pretrained on the COCO dataset. The model predicts bounding boxes, class labels and segmentation masks for detected objects. The network automatically uses GPU if available, while `eval()` disables training-specific layers. Input images are converted into tensors of shape $\mathbb{R}^{3\times H\times W}$, and `unsqueeze(0)` adds the batch dimension $\mathbb{R}^{1\times 3\times H\times W}$. During inference, the model outputs dictionaries containing "boxes", "labels", "scores" and "masks". Low-confidence detections are filtered, and soft masks $M_i \in [0,1]^{H\times W}$ are thresholded into binary masks using $\hat{y}_{uv} > 0.3$. Distinct colors are assigned to each instance for visual separation, and an RGB overlay is generated using alpha blending:

$$I_{overlay} = \alpha M + (1 - \alpha) I$$

Final visualization displays original image, predicted masks and segmentation overlay side-by-side.

```
model = torchvision.models.detection.maskrcnn_resnet50_fpn(weights="DEFAULT")
model.eval()
image = Image.open('images/cats_dogs.jpg').convert("RGB")
img_tensor = transforms.ToTensor()(image).unsqueeze(0).to(device)
with torch.no_grad(): outputs = model(img_tensor)
scores, masks = outputs[0]['scores'], outputs[0]['masks']
labels = outputs[0]['labels']
keep = scores > 0.7
masks = masks[keep].cpu().numpy()
h, w = masks.shape[-2:]
combined_mask = np.zeros((h, w, 3)) # RGB canvas
cmap = plt.get_cmap('gist_rainbow')
for i, m in enumerate(masks):
```
