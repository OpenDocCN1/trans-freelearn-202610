```
result = client.images.edit(model="gpt-image-1", image=open(image, "rb"),
    mask=open("images/mask.png", "rb"),
    prompt="Replace edges with artistic glowing
lines")
return Image.open(io.BytesIO(base64.b64decode(result.data[0].b64_json)))
```

This demonstrates a hybrid workflow in which classical image processing provides structural guidance while generative AI synthesizes new visual content. The Gradio application combines OpenCV edge detection with GPT Image inpainting to automatically replace image edges with AI-generated glowing artistic contours through an interactive web interface.

```
import Gradio as gr
gr.Interface(fn=edge_inpaint, inputs=gr.Image(type="filepath"),
    outputs=gr.Image(),
    title="OpenCV + GenAI Hybrid").launch()
```

The next figure shows the Gradio interface for an edge-guided image genration:

![](bbox=[0.17, 0.4, 0.8, 0.68])

Figure 13.40 – Gradio app for edge-guided image editing with GPT image

# Image inpainting

Inpainting fills missing or masked image regions while preserving surrounding content.

```
result = client.images.edit(model="gpt-image-1", image=img_f, mask=mask_f,
    prompt="Fill the masked region realistically")
```

The mask identifies regions to be modified, and the model synthesizes visually consistent content, as demonstrated by the Gradio app shown below (refer to GitHub).