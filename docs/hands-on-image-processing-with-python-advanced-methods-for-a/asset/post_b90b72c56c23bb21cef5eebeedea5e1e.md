
The model returns:

```
boxes, scores, labels = outputs["boxes"], outputs["scores"], outputs["labels"]
```

Conceptually, Image → RetinaNet → Box_i, Class_i, Score_i, where each detection consists of a bounding box, class label, and confidence score.

**Multi-Scale Detection with FPN:** Objects appear at different sizes within an image. RetinaNet uses a Feature Pyramid Network to combine information from multiple resolutions:

$P_l = Conv(C_l) + Up(P_{l+1})$

allowing small and large objects to be detected simultaneously.

**Confidence Thresholding:** Low-confidence detections are removed.

```
threshold = 0.6
keep = scores >= threshold
boxes, scores, labels = boxes[keep], scores[keep], labels[keep]
```

This reduces false positives and improves visualization quality.

**Visualization:** Each detected class is assigned a unique color and bounding boxes and labels are drawn:

```
colors = generate_colors(len(unique_labels))
cv2.rectangle(img, (x1,y1), (x2,y2), color, 3)
text = f"{class_name} ({score:.2f})"
```

The final output displays object locations, predicted categories, and confidence scores in a visually interpretable form.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_652_0.png)

**Why RetinaNet Works:** RetinaNet combines deep feature extraction (ResNet50), multi-scale representation learning (FPN) and robust handling of class imbalance (*Focal Loss*). These components enable accurate detection of multiple objects across a wide range of scales while maintaining high inference speed.

**Advantages:** RetinaNet achieves high detection accuracy while remaining relatively efficient, thanks to its **Feature Pyramid Network (FPN)** for multi-scale object detection and **Focal Loss**, which focuses learning on difficult examples and mitigates foreground–background class imbalance. As a pretrained *COCO* model, it can detect a wide variety of object categories without additional training. Compared with *YOLO* (another popular object detection model to be introduced shortly), which prioritizes real-time inference speed, *RetinaNet* places greater emphasis on detection accuracy, particularly for small and challenging objects, making it well-suited for high-quality detection tasks.

**Limitations:** Small, heavily occluded, or visually ambiguous objects may still be missed or misclassified, and inference is generally slower than lightweight detectors such as YOLO variants. Since the model is pretrained on COCO, performance can degrade on domain-specific objects unless it is fine-tuned on a task-specific dataset.

## Object detection and instance segmentation with mask R-CNN

While object detectors such as RetinaNet predict only bounding boxes, **Mask R-CNN** extends detection by additionally generating a **pixel-level segmentation mask** for every detected object (as we have seen in the segmentation chapter). Thus, each instance is represented by a class label, confidence score, bounding box, and segmentation mask, enabling precise localization of object boundaries.

**Mask R-CNN is a two-stage object detector:** the first stage (**Region Proposal Network, RPN**) generates candidate object regions, while the second stage classifies each region, refines its bounding box, and predicts a pixel-wise segmentation mask for every detected instance.

In this example, we use a **pretrained Mask R-CNN model** (`maskrcnn_resnet50_fpn`) trained on the **COCO dataset**. The model employs a ResNet-50 backbone with a **Feature Pyramid Network (FPN)** for multi-scale feature extraction and predicts object masks in parallel with object classification and localization. For an input image, the network returns:

```
outputs = model([img_tensor])[0]
boxes, scores, labels, masks = outputs["boxes"], outputs["scores"],
outputs["labels"], outputs["masks"]
```

Low-confidence detections are removed using a confidence threshold (`keep = scores >= 0.50`). Each surviving detection is assigned a unique color and visualized using four complementary views:

- **Original Image** – input scene
- **Mask Overlay** – instance segmentation masks
- **Object Contours** – extracted object boundaries
- **Final Detection View** – masks, contours, bounding boxes, class labels, and confidence scores combined

The segmentation mask is obtained by thresholding the predicted probability map, where the colored masks are blended with the original image to create transparent overlays.

```
mask = (mask[0] > 0.5).astype(np.uint8)
final_result = np.where(mask[:, :, None],
                        final_result * (1 - alpha) + overlay * alpha, final_result)
```

Object boundaries are extracted using OpenCV contours. A detection summary table (refer to GitHub) is also generated showing the predicted class, confidence score, and segmented area for each instance.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_654_0.png)

*Figure 12.37 – Object detection and instance segmentation with Mask R-CNN*

Mask R-CNN can be viewed as performing **object detection and semantic segmentation simultaneously**. First, the detector identifies *where* objects are located, and then a dedicated mask branch predicts *which* pixels belong to each object. Instead of merely drawing a box around a bicycle, Mask R-CNN can precisely delineate the frame, wheels, and handlebars, producing a much richer scene understanding. The visualization demonstrates how Mask R-CNN progresses from raw image data to instance masks, contours, and final detections. Such pixel-level localization is particularly useful in applications such as medical image analysis, autonomous driving, robotics, video surveillance, and image editing.

**Exercise:** The pretrained model may occasionally produce multiple overlapping detections for the same object (for example, the object cycle here is detected twice, once for each wheel). Extend the implementation by applying **Non-Maximum Suppression (NMS)** (or class-wise NMS; use `torchvision.ops.nms`) before visualization to remove redundant detections and obtain cleaner instance segmentation results.

While object detection and instance segmentation localize and delineate visual objects, OCR extends the same localization principle to text regions and then performs character recognition, enabling machines to read textual information directly from images, as we shall explore in the next section.

## Optical Character Recognition (OCR) and Vision-Language Text Extraction Pipelines

This section extends the previous discussion on object detection by introducing **Optical Character Recognition (OCR)** as a post-detection or standalone recognition module. OCR bridges the gap between **visual perception and machine-readable text**, enabling systems to interpret structured documents, scene text, and identity-related visual content such as invoices, signboards, checks, and license plates.

### Problem Formulation: From Pixels to Text:

Given an input image $I \in \mathbb{R}^{H\times W\times 3}$, OCR aims to learn a mapping:

$$F_{OCR}: I \rightarrow \mathcal{T},$$

where $\mathcal{T}=t_1,t_2,\ldots,t_n$ is a sequence of recognized text tokens. Classical OCR systems decompose this into two stages:

- **Text detection:** locate regions $R_i \subset I$
- **Text recognition:** decode each region into a string $t_i=f_\theta(R_i)$

Modern systems integrate both into unified deep learning pipelines.

### Baseline OCR with Tesseract

Tesseract OCR is a traditional **LSTM-based recognition engine** combined with layout analysis. It performs best when text is well-structured (documents, invoices) and preprocessing improves contrast and noise levels. It supports page segmentation modes (`--psm`) and OCR Engine Modes (`--oem`).

**Mathematical View:** Tesseract approximates $\arg\max_T P(T\mid I)$ using sequence modeling:

$$P(T\mid I)=\prod_{i=1}^{n}P(t_i\mid I,t_{<i})$$

**Implementation - Invoice Text Extraction:** The image is first converted to grayscale, and then OCR is applied:

```
gray = cv2.cvtColor(bill, cv2.COLOR_BGR2GRAY)
text = pytesseract.image_to_string(gray, config=r'--oem 3 --psm 4')
```

Grayscale conversion reduces photometric complexity, while `--psm 4` assumes a **sparse column-based layout**, suitable for invoices.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_655_0.png)

*Figure 12.38 – Extracting text with OCR using Tesseract*
