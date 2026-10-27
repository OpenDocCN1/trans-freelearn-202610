```
pred_class = outputs.logits.argmax()  
score = outputs.logits[0, pred_class]  # Saliency map  
score.backward()  
saliency = (inputs["pixel_values"].grad.abs().mean(dim=1)[0])  
overlay = cv2.addWeighted(image, 0.6, heatmap, 0.4, 0)  # Overlay heatmap on image
```

This approach uses **gradient-based saliency** to reveal which image regions most strongly influenced the Swin Transformer’s classification decision, making it an example of **Explainable AI (XAI)** unlike the ViT attention-rollout visualization. The next figure shows the resulting saliency maps and attention overlays.

![](bbox=[0.093, 0.294, 0.862, 0.458])

*Figure 12.12 – Saliency map with Swin Transformer*

Attention mechanisms allow deep networks to allocate computational resources selectively, much like human visual perception. CNN attention modules improve feature selection, while Vision Transformers and Swin Transformers employ self-attention to model long-range image relationships and generate highly interpretable classification decisions.

# Few-shot learning

Traditional supervised image classification requires hundreds or thousands of labeled examples per class. In contrast, **few-shot learning (FSL)** aims to learn new categories from only a handful of labeled examples. Formally, in an *N*-way *K*-shot task, the model must classify among *N* classes using only *K* labeled examples per class. FSL is particularly important in domains where annotated data is scarce, such as medical imaging, remote sensing, and industrial inspection.

Intuitively, few-shot learning resembles human learning: after seeing only a few examples of a new bird species, a person can often recognize similar birds in future images. Modern FSL systems achieve this capability by learning transferable visual representations rather than memorizing class labels.

# Zero-shot learning with CLIP

The CLIP model jointly learns image and text representations using contrastive learning on hundreds of millions of image-text pairs. During inference, classification can be performed *without any task-specific training* by comparing an image with textual prompts describing candidate classes (that’s why CLIP is a zero-shot model).