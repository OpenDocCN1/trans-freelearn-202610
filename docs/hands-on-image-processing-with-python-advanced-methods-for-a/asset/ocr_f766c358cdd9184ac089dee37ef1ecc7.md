# Visual Question Answering (VQA)

Vision models can answer arbitrary questions about an image. The function `analyze()` in the next code snippet sends an image and a natural-language question to a multimodal GPT model and returns the model’s visual understanding of the image.

```
```python
 def analyze(image, question):
     with open(image, "rb") as f: b64 = base64.b64encode(f.read()).decode()
     response = client.responses.create(model="gpt-4.1-mini",
         input=[{"role": "user", "content": [{"type": "input_text",
             "text": question},
             {"type": "input_image", "image_url": f"data:image/jpeg;base64,{b64}"}]
         }])
     return response.output_text
```
```

The Gradio app interface demonstrating VQA is shown in the following figure:

![](bbox=[0.253, 0.378, 0.812, 0.631])

*Figure 13.46 – Gradio App for Visual Question-Answering System with GPT-Image*

> **Note**
> 
> The three essential steps above: **encode image → send image and question to GPT → return the answer.** This capability is known as **Visual Question Answering (VQA)**, demonstrated by the next Gradio app.

# Automated industrial inspection (detecting manufacturing defects)

GPT Vision can function as a quality-control assistant. The next code uses a multimodal GPT model as an automated quality-control inspector to analyze an image of capsules for *manufacturing defects*.