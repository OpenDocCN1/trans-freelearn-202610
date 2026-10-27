The next code performs zero-shot image classification using a pretrained CLIP model by comparing the input image with multiple text descriptions and selecting the class whose text embedding is most similar to the image embedding, while also visualizing the regions that influenced the prediction. CLIPModel loads the pretrained CLIP neural network, while CLIPProcessor automatically tokenizes the text prompts and preprocesses the input image into the format required by the model.

```
```python
# Load pretrained CLIP
from transformers import CLIPProcessor, CLIPModel
model = CLIPModel.from_pretrained("openai/clip-vit-base-patch32")
classes = ["a photo of a dog", "a photo of a horse", "a photo of a parrot"]
outputs = model(**inputs)
logits = outputs.logits_per_image # Image-text similarities
probs = logits.softmax(dim=1)
```
```

Instead of predicting from a fixed classifier head, CLIP computes similarities between image and text embeddings. The class whose textual description is most similar to the image embedding is selected as the prediction. To visualize the model’s reasoning, Grad-CAM can be applied to the visual encoder:

```
```python
logits[0, class_idx].backward() # Grad-CAM
cam = (acts * weights.unsqueeze(1)).sum(dim=2)
```
```

The resulting heatmap highlights image regions that contribute most strongly to the selected text prompt, providing an interpretable explanation of the zero-shot prediction. The next figure shows the resulting image-text similarity scores and classification probabilities.

![](bbox=[0.143, 0.53, 0.896, 0.697])

*Figure 12.13 – Zero-shot image classification with CLIP*

# Few-shot learning with DINOv2

DINOv2 is a self-supervised Vision Transformer trained without class labels. Instead of learning explicit categories, DINOv2 learns powerful visual embeddings that can be reused for downstream tasks. A simple few-shot classifier can be constructed using class prototypes: In this example, the classification head is removed and the network is used purely as a feature extractor that maps each image to a high-dimensional embedding vector.

```
```python
model = timm.create_model('vit_base_patch14_dinov2.lvd142m', pretrained=True)
model.reset_classifier(0)  # use embeddings
```
```