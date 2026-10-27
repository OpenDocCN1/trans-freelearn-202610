
**Output Interpretation:** The extracted text reconstructs structured billing information such as invoice metadata (date, ID), line-item parsing (food items, quantity, price) and final totals.

This demonstrates a key limitation of classical OCR: it does not inherently understand structure—only text sequences.

**Multilingual Scene Text Recognition:** Scene text (e.g., signboards) introduces non-Latin scripts, noise/distortion, and variable lighting.

**Example: Bengali + English Signboard OCR**

```
text = pytesseract.image_to_string(img, lang='ben', config=config)
```

Multilingual OCR extends recognition ℱ * OCR: I → 𝒯 * lang, where language-conditioned decoding becomes: P(T | I, L) with L as language prior. Unlike document OCR, multilingual scene-text recognition must simultaneously handle diverse scripts, complex backgrounds, perspective distortions, variable illumination, and language identification, making the recognition task substantially more challenging.

Multilingual OCR handles multiple scripts by combining language-specific character dictionaries and language models, enabling the decoder to recognize and disambiguate text from different languages within the same image. Output includes mixed-language detection: English tokens (e.g., “CCTV”, “PARKING”) and Bengali script recognition. This highlights dependency on language models and trained character dictionaries.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_656_0.png)

*Figure 12.39 – Multilingual scene text recognition with Tesseract*

Having explored classical OCR with Tesseract, we now move to deep-learning-based OCR with PaddleOCR, which offers greater robustness in real-world scenarios.

## Deep Learning OCR: PPOCR (PaddleOCR)

*PaddleOCR introduces a three-stage deep pipeline:*

- Text Detection (DBNet)
- Angle Classification
- Text Recognition (SVTR / CRNN variants)

**Key Advantage:** Unlike Tesseract, it learns detection and recognition jointly, is robust to curved and rotated text, and offers better performance on real-world images.

```
ocr = PaddleOCR(use_angle_cls=True, lang='en')
result = ocr.ocr(image_path, cls=True)
```

Extracted components are bounding boxes $B_i$, text predictions $t_i$, and confidence scores $s_i$.

**Visualization:** The system overlays $(B_i, t_i, s_i)$ onto the image using draw_ocr, enabling interpretability.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_657_0.png)

*Figure 12.40 – Extracting texts from a bank check with PPOCR*

*PaddleOCR explicitly models $P(T, B \mid I) = P(B \mid I) \cdot P(T \mid B, I)$ by decoupling detection and recognition for stability.*

## ALPR (Automatic License Plate Recognition): End-to-End OCR via detection + recognition

Automatic License Plate Recognition (ALPR) is a specialized OCR problem in which the text region must first be localized before character recognition can be performed. Consequently, modern ALPR systems employ a two-stage pipeline:

$I \xrightarrow{\text{YOLOv5}} R_i \xrightarrow{\text{OCR}} t_i,$

where $R_i$ denotes the detected license-plate region and $t_i$ the recognized plate text. In this example, plate localization is performed using YOLOv5, a single-stage object detector that predicts bounding boxes and class probabilities in a single forward pass, enabling real-time detection.

The pretrained YOLOv5m6 model is fine-tuned via transfer learning on the Car Plate Detection Kaggle dataset containing 433 vehicle images annotated with license-plate bounding boxes. During training, the detector learns to predict plate locations $(x_{\min}, y_{\min}, x_{\max}, y_{\max})$ by optimizing localization, objectness, and classification losses. The detected plate region is then cropped and passed to Tesseract OCR, which extracts the alphanumeric license number, producing a complete end-to-end ALPR system.

### Visualizing the Final Result:

The recognized license number is rendered on the detected image.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_660_0.png)

*Figure 12.42 – License plate detection with YOLOv5 + Tesseract*

The complete ALPR pipeline can therefore be summarized as Image → YOLOv5 (License Plate ROI) → Tesseract (Plate Number). By combining deep-learning-based object detection with a **modular perception pipeline**, the system automatically localizes vehicle plates and extracts their alphanumeric identifiers, forming the basis of modern intelligent transportation and surveillance systems.

OCR systems have evolved from Handcrafted rules → CNN/LSTM pipelines → Vision Transformers. Modern OCR is no longer just character recognition—it is now a **vision-language understanding task**, tightly integrated with object detection, scene understanding, and generative modeling.

## Scene text recognition with EasyOCR

Having built a complete ALPR pipeline using YOLO and Tesseract, let’s now explore EasyOCR, a lightweight deep-learning-based OCR system (based on CNN feature extraction and sequence decoding) that can perform robust scene-text detection and recognition in just a few lines of code.

**Inference Model:** *I* → *CNN* → *SequenceDecoder* → *T*. It outputs bounding box, text, and confidence score.

```python
import easyocr
reader = easyocr.Reader(['en'])
img = cv2.imread("images/car.png")
results = reader.readtext(img)
```

The license plate texts extracted are shown in the next figure.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_660_1.png)

*Figure 12.43 – License plate recognition with EasyOCR*

real-time landmark detectors capable of estimating dense facial and hand keypoints, making it an ideal framework for interactive vision applications.

In this section, we use *MediaPipe*’s Tasks API to build Snapchat-style face filters and visualize hand landmarks. These examples demonstrate how modern landmark detection can be used for facial augmentation, gesture analysis, virtual accessories, and human-computer interaction.

## Face landmark detection and Snapchat-Style filters

The MediaPipe Face Landmarker predicts a dense facial mesh consisting of 468 landmarks:

$$L = \{(x_i, y_i, z_i)\}_{i=1}^{468}$$

where each landmark represents a semantically meaningful facial location such as the eyes, eyebrows, nose, lips, or jawline. Given an image $I$, the detector learns a mapping $f(I) \to L$, which estimates normalized landmark coordinates in image space. These landmarks provide a geometric representation of the face that is invariant to scale and moderate pose changes.

In the implementation, *MediaPipe*’s Face Landmarker model is initialized through the *Tasks API*:

```python
from mediapipe.tasks.python import vision
options = vision.FaceLandmarkerOptions(base_options=base_options,
        output_face_blendshapes=False,
        output_facial_transformation_matrixes=True, num_faces=1)
self.detector = vision.FaceLandmarker.create_from_options(options)
```

which loads the face landmark model and restricts inference to the most prominent face in the image. Before inference, the *OpenCV* image is converted into *MediaPipe*’s internal image representation:

```python
mp_image = mp.Image(image_format=mp.ImageFormat.SRGB, data=frame)
result = self.detector.detect(mp_image)
```

This is the core inference step which predicts the landmarks. Specific landmark indices are selected to define facial regions of interest: left eye (33), right eye (263), nose tip (1), mouth corners (61, 291). The landmarks serve as anchor points for positioning virtual objects such as sunglasses, moustaches, and cartoon noses. For example, the eye landmarks

```python
left_eye, right_eye = face_landmarks[33], face_landmarks[263]
```

provide stable reference points for positioning sunglasses, while the nose tip and mouth corners do not.

To place a filter correctly, the inter-eye distance is used as a scale reference:

$$d = \sqrt{(x_r - x_l)^2 + (y_r - y_l)^2},$$

where $(x_l, y_l)$ and $(x_r, y_r)$ denote the left and right eye coordinates. The overlay width is then computed as $W_f = \alpha d$, where $\alpha$ is an empirically chosen scaling factor, e.g., the sunglasses width is computed as:

```python
glass_w = int(1.4 * abs(rx - lx))
```
