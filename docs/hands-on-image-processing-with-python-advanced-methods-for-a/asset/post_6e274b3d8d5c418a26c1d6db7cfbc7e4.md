
+   - **Image preprocessing:** The input image is read with OpenCV, converted from BGR to RGB using `cv2.cvtColor()`, and transformed into the tensor format required by the networks through the `preprocess()` function.
+   - **Inference:** Predictions are performed inside a `torch.no_grad()` block to reduce memory usage and improve execution efficiency. Each model produces segmentation logits through `model(input_tensor)["out"]`.
+   - **Resolution alignment:** When necessary, the logits are resized to a common spatial resolution using `F.interpolate()` so that predictions from different models can be combined consistently.
+   - **Probability estimation:** The aligned logits are converted into class-probability maps using `F.softmax(..., dim=1)`. For each model, a pixel-wise segmentation mask is then obtained using `torch.argmax()`, and the resulting masks are stored for later visualization.
+   - **Ensemble voting:** The probability maps from all models are collected in `prob_maps`, combined using `torch.stack()`, and averaged with `torch.mean()` to perform soft-voting ensemble fusion. The final ensemble segmentation mask is obtained by applying `torch.argmax()` to the averaged probability map.
+   - **Visualization:** The individual model masks and the final ensemble mask are displayed side-by-side, enabling visual comparison between the predictions of the constituent models and the consensus ensemble result.

Now refer to the next code snippet:

```
import torch, torch.nn.functional as F

from torchvision.models.segmentation import (
    fcn_resnet50, FCN_ResNet50_Weights,
    deeplabv3_resnet50, DeepLabV3_ResNet50_Weights,
    deeplabv3_mobilenet_v3_large, DeepLabV3_MobileNet_V3_Large_Weights,
    lraspp_mobilenet_v3_large, LRASPP_MobileNet_V3_Large_Weights)

models_dict = {"FCN-R50": \
    fcn_resnet50(weights=FCN_ResNet50_Weights.COCO_WITH_VOC_LABELS_V1),
    "DeepLab-R50": deeplabv3_resnet50( \
        weights=DeepLabV3_ResNet50_Weights.COCO_WITH_VOC_LABELS_V1),
    "DeepLab-Lite": deeplabv3_mobilenet_v3_large( \
        weights=DeepLabV3_MobileNet_V3_Large_Weights.COCO_WITH_VOC_LABELS_V1),
    "LRASPP": lraspp_mobilenet_v3_large( \
        weights=LRASPP_MobileNet_V3_Large_Weights.COCO_WITH_VOC_LABELS_V1)}

for name in models_dict:
    models_dict[name] = models_dict[name].to(DEVICE).eval()

img_rgb = cv2.cvtColor(cv2.imread("images/cycle.jpg"), cv2.COLOR_BGR2RGB)
```

```
```python
input_tensor = preprocess(img_rgb)  # refer to GitHub for preprocess()
prob_maps,individual_masks = [], []
with torch.no_grad():
    for name, model in models_dict.items():
        probs = F.softmax(model(input_tensor)["out"], dim=1)
        prob_maps.append(probs)
        individual_masks.append(
            torch.argmax(probs,dim=1).squeeze().cpu().numpy())
ensemble_prob = torch.mean(torch.stack(prob_maps), dim=0)  # Soft Voting
ensemble_mask = torch.argmax(ensemble_prob, dim=1).squeeze().cpu().numpy()
# Plot individual and ensemble masks here side-by-side
```
```

If you run the above code, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_519_0.png)

*Figure 10.17 – Image segmentation using ensemble voting with pretrained deep learning models*

Although FCNs pioneered semantic segmentation, they often struggle with boundary precision and multi-scale context modeling. The *DeepLab* family addresses these challenges through atrous convolution and *Atrous Spatial Pyramid Pooling* (ASPP), enabling richer contextual understanding without sacrificing spatial resolution.

## Atrous convolution and the DeepLab family

**DeepLab** improves FCNs using atrous (dilated) convolution:

$$y[i] = \sum_k x[i + r \cdot k], w[k],$$

where $r$ is the dilation rate, enabling larger receptive fields without reducing spatial resolution.

Multi-scale context is captured using Atrous Spatial Pyramid Pooling (ASPP):

$$\mathrm{ASPP}(x) = \bigoplus_{r \in \{r_1,r_2,r_3,r_4\}} \mathrm{Conv}_r(x),$$

which aggregates features at multiple dilation scales. DeepLab variants progressively evolved from DeepLabV1 (CRF refinement), DeepLabV2 (atrous convolution), DeepLabV3 (ASPP), to DeepLabV3+ (encoder–decoder refinement).

## DeepLabV3 with `torchvision.models.segmentation`

The DeepLabV3 inference pipeline is analogous to the previous FCN/Torchvision segmentation example and is left as an exercise to the reader (refer to the accompanying GitHub repository). Here, a pretrained `deeplabv3_resnet101()` model is used to generate a semantic segmentation map from an input image

using Torchvision preprocessing, forward inference, logits upsampling with F.interpolate(), and pixel-wise prediction using torch.argmax(). The next figure shows DeepLabV3 segmentation outputs and overlays:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_520_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_520_1.png)

Figure 10.18 - Image segmentation with DeepLabV3 from torchvision.models.segmentation

DeepLabV3 is not limited to the PyTorch ecosystem. The following example demonstrates the same semantic segmentation task using a pretrained DeepLabV3 model from the TensorFlow Model Garden, allowing readers to compare the workflow across two popular deep learning frameworks.

## DeepLabV3 with tensorflow Model Garden

The code demonstrates semantic segmentation using a pretrained DeepLabV3 model from the TensorFlow Model Garden by downloading and loading a frozen inference graph trained on the Cityscapes dataset, wrapping it into a callable TensorFlow inference function, and preprocessing input images through resizing and NumPy conversion to match the model's expected input format. It then performs forward inference to obtain pixel-wise semantic predictions, resizes the predicted segmentation maps back to the original image resolution, maps class indices to semantic labels and RGB colors for visualization, and optionally loads ground-truth masks for qualitative or quantitative evaluation of segmentation performance. Now, refer to the following code snippet:

```
tar = tarfile.open("models/deeplab_model.tar.gz")
for m in tar.getmembers():
    graph_def = tf.compat.v1.GraphDef.FromString(
        tar.extractfile(m).read())
    tar.close()
model = tf.function(lambda x:
    tf.import_graph_def(graph_def, input_map={"ImageTensor:0": x},
                        return_elements=["SemanticPredictions:0"]))

def segment(img_file):
    img = Image.open(img_file).convert("RGB")
    h, w = img.size[1], img.size[0]
    inp = np.asarray(img.resize((2049,1025)))[None]
    seg = model(inp)[0].numpy()[0]    # inference
    seg = cv2.resize(seg,(w,h),interpolation=cv2.INTER_NEAREST)
    return img, seg
```

The function segment() performs preprocessing, runs the DeepLab model to generate pixel-wise segmentation, loads ground truth, and returns image, prediction, and GT. Run inference as shown in the next code snippet to obtain a figure like the following one:

```
img, seg = segment("images/mit_driveseg_sample.png", \
"images/mit_driveseg_sample_gt.png")
# Plot img, seg, overlay and gt side-by-side here
```

If you run the above code and plot the original image, predicted segmentation mask and the overlay images side-by-side you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_521_0.png)

Figure 10.19 – Segmenting a drive-seg sample image with a pretrained DeepLabV3 model from tensorflow model garden

If you plot the original image, ground-truth segmentation mask and the overlay images side-by-side you should obtain a figure like the following (compare the figures):

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_521_1.png)

Figure 10.20 – The original drive-seg sample image and the ground-truth mask

Next let’s evaluate the model to quantify how good it performs: the next code evaluates semantic segmentation predictions by comparing predicted labels with ground-truth masks using an evaluate_single() function, harmonizing label mappings through logical masking and indexing so that prediction classes match the annotation scheme. It computes pixel accuracy over valid (non-void) pixels using masked summation, constructs a confusion matrix with sklearn.metrics.confusion_matrix() to capture class-wise prediction statistics, and derives per-class intersection and union values from the matrix using diagonal and row/column sums to compute IoU scores. Finally, it calculates mean IoU across classes with np.mean() and presents detailed class-wise evaluation results in a formatted table using tabulate().

```
def evaluate_single(seg_map, ground_truth):
    seg_map[np.logical_or(seg_map==14,seg_map==15)] = 13
```
