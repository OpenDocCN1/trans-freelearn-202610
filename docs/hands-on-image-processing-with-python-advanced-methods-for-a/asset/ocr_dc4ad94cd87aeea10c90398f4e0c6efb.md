+   - **Multi-Stage Hierarchical Encoder** Produces feature maps at multiple resolutions. The encoder outputs are combined (via multi-scale feature fusion) as:
+   
+     $$F = \operatorname{concat}(F_1, F_2, F_3, F_4)$$
+   
+   - **No Positional Encoding** (a key innovation—spatial information is implicitly preserved).
+   
+   - **Lightweight MLP Decoder** Efficiently fuses multi-scale features, so that the final segmentation prediction is:
+   
+     $$\hat{Y} = \operatorname{MLP}(F)$$

This design avoids heavy decoders (e.g., U-Net style), preserves spatial detail and reduces computation significantly. Thus, a SegFormer can be understood as a *CNN-like hierarchical feature extractor implemented using transformers, combined with a simple yet powerful decoder.*

Let’s now demonstrate semantic segmentation with the next code snippet which uses the pretrained model **nvidia/segformer-b0-finetuned-ade-512-512**. Here **processor** (AutoImageProcessor) converts the image into normalized tensors, handles resizing, padding, and batching.

Also performs **post-processing**, whereas **model** (AutoModelForSemanticSegmentation) implements SegFormer architecture and outputs **logits** of shape: `[1, C, H′, W′]` where `C` = number of classes.

The `post_process_semantic_segmentation()` method *upsamples logits* to original image size, applies **argmax over class dimension** and returns a **pixel-wise class map**:

$$\hat{Y}(x,y) = \operatorname{argmax}_c\ \operatorname{logits}_c(x,y)$$

```
from transformers import AutoImageProcessor,AutoModelForSemanticSegmentation
model_name = "nvidia/segformer-b0-finetuned-ade-512-512"
processor = AutoImageProcessor.from_pretrained(model_name)
model = AutoModelForSemanticSegmentation.from_pretrained(model_name)

def run_segmentation(image_path):
    image = Image.open(image_path).convert("RGB")
    image_np = np.array(image)
    inputs = processor(images=image, return_tensors="pt").to(device)
    with torch.no_grad(): outputs = model(**inputs)
    segmentation = processor.post_process_semantic_segmentation(outputs, \
            target_sizes=[image.size[::-1]])[0]
    seg_color = colorize(segmentation)
    overlay = (0.4 * image_np + 0.6 * seg_color).astype(np.uint8)
    # Plot image_np, seg_color and detr_overlay side-by-side here

run_segmentation("images/cricket.jpg")
```

If you run the above code, you should obtain a figure like the following: