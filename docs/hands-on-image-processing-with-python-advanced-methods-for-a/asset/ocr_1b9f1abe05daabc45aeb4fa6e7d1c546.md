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