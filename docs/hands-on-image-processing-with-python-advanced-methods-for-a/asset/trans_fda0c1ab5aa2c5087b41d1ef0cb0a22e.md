

```python
result = client.images.edit(model="gpt-image-1", image=open(image, "rb"),
    mask=open("images/mask.png", "rb"),
    prompt="Replace edges with artistic glowing lines")
return Image.open(io.BytesIO(base64.b64decode(result.data[0].b64_json)))
```

这展示了一种混合工作流：传统图像处理提供结构引导，而生成式 AI 合成新的视觉内容。该 Gradio 应用结合了 OpenCV 边缘检测与 GPT Image 修复功能，通过交互式 Web 界面自动将图像边缘替换为 AI 生成的发光艺术轮廓。

```python
import gradio as gr

gr.Interface(fn=edge_inpaint, inputs=gr.Image(type="filepath"),
    outputs=gr.Image(),
    title="OpenCV + GenAI Hybrid").launch()
```

下图展示了用于边缘引导图像生成的 Gradio 界面：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_726_0.png)

图 13.40 – 使用 GPT 图像进行边缘引导图像编辑的 Gradio 应用

## 图像修复（Image inpainting）

修复功能可在保留周围内容的同时，填补缺失或被遮罩的图像区域。

```python
result = client.images.edit(model="gpt-image-1", image=img_f, mask=mask_f,
    prompt="Fill the masked region realistically")
```

遮罩用于标识需要修改的区域，模型会合成视觉上一致的内容，如下方 Gradio 应用所示（参见 GitHub）。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_727_0.png)

图 13.41 – 使用 Gradio 和 GPT-image 模型（OpenAI API）的 AI 修复工具

典型应用包括物体移除、划痕去除、损坏修复、背景补全和内容替换。

## AI 风格迁移

GPT Image 可以将图像转换为所需的艺术风格，同时保留不同程度的结构信息。下一个 Gradio 演示中的 `build_prompt()` 函数会根据结构保持参数动态调整提示词。低数值允许创造性重新诠释，而高数值则保留几何形状、物体布局和透视。`style_transfer()` 函数使用 GPT Image Editing 将选定的艺术风格应用于输入图像，同时强制实现所需的结构保持水平。

```python
client = OpenAI(api_key=api_key)

def build_prompt(style, strength):
    if strength < 0.3:
        return f"""Transform this image into {style} style.
        Feel free to creatively reinterpret the scene."""
    else:
        return (f"Apply {style} style. Preserve composition,"
                f"geometry, object positions and perspective."
                f"Only modify colors, textures and lighting.")

def style_transfer(image_path, style, strength):
    prompt = build_prompt(style, strength)
    result = client.images.edit(model="gpt-image-1",
                                image=open(image_path, "rb"), prompt=prompt)
    img_bytes = base64.b64decode(result.data[0].b64_json)
    return Image.open(io.BytesIO(img_bytes))
```

用于应用吉卜力风格的 Gradio 应用界面如下所示：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_728_0.png)

图 13.42 – 使用 GPT 图像从输入图像生成吉卜力风格图像的 Gradio 应用

支持的风格包括动漫、水彩、油画、赛博朋克、铅笔素描、吉卜力工作室和梵高。这种方法提供了一种可控的风格迁移形式，无需单独的风格迁移网络。

## 人脸修复

视觉模型和图像生成模型可以组合成一个两阶段的修复流水线。

### 阶段 1：图像评估

GPT Vision 首先分析图像质量。该模型会估计输入旧照片的模糊程度、噪声、划痕和修复需求。

```python
response = client.responses.create(model="gpt-4.1",
    input=[{"role": "user", "content": [
        {"type": "input_text", "text": """Analyze this image. Estimate:
blur level,
    noise level, scratches, restoration needs"""},
        {"type": "input_image", "image_url": f"data:image/jpeg;base64,{base64.b64encode(img_bytes).decode()}"}
    ]}])
```

```python
analysis = response.output_text
print("\nGPT Vision Analysis:")
print(analysis)
```

> **摘要**：
> 这是一张严重模糊的复古图像，噪声较低，无明显划痕。修复工作主要应侧重于提升清晰度、对比度，并重建关键面部特征，以恢复照片的清晰度和个性。

### 阶段 2：修复

```python
result = client.images.edit(model="gpt-image-1", image=[("old_photo.jpg", img_bytes)], prompt="""Restore this photograph. Remove blur. Remove noise.

Improve facial detail. Preserve identity.""")
```

模型在保留原始身份和整体外观的同时，增强了面部细节。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_729_0.png)

图 13.43 – 使用 GPT Vision 进行人脸修复

应用包括历史照片修复、家庭照片增强、档案数字化和人脸重建。

## 多图像合成

GPT Image 可以联合推理多张输入图像，如下一段代码所示：

```python
result = client.images.edit(model="gpt-image-1",
    image=[open("images/beach.png", "rb"), open("images/whale.jpg", "rb")],
    # image=[open("images/giraffe.jpg", "rb"), open("images/bird.jpg", "rb")]
    prompt="Combine these into a realistic product scene")
```

模型将来自多个来源的视觉概念（鲸鱼与海滩、长颈鹿与鸟类）融合成一个连贯的构图，如下图所示。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_730_0.png)

图 13.44 – 使用 GPT-image 进行多图像合成

应用包括广告设计、产品可视化、概念艺术和合成场景创作。

## 电商产品增强

产品图像可以自动转换为专业的营销照片。

```python
result = client.images.edit(model="gpt-image-1", image=f,
    prompt="""Place this product in a professional
    studio background with soft lighting""")
```

模型通过改善背景、光照、反射和呈现质量来执行虚拟摄影，如下一个 Gradio 应用所示。

### 电商产品生成器

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_730_1.png)

图 13.45 – 使用 GPT-image 进行电商产品增强的 Gradio 应用

## 视觉问答（VQA）

视觉模型可以回答关于图像的任意问题。下一段代码中的 `analyze()` 函数将图像和自然语言问题发送给多模态 GPT 模型，并返回模型对图像的视觉理解。

```python
def analyze(image, question):
    with open(image, "rb") as f:
        b64 = base64.b64encode(f.read()).decode()
    response = client.responses.create(model="gpt-4.1-mini",
        input=[{"role": "user", "content": [
            {"type": "input_text", "text": question},
            {"type": "input_image", "image_url": f"data:image/jpeg;base64,{b64}"}
        ]}])
    return response.output_text
```

展示 VQA 的 Gradio 应用界面如下图所示：

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_731_0.png)

图 13.46 – 使用 GPT-Image 的视觉问答系统 Gradio 应用

> **注意**
>
> 上述三个基本步骤为：**编码图像 → 将图像和问题发送给 GPT → 返回答案。** 这种能力被称为**视觉问答（VQA）**，下一个 Gradio 应用对此进行了演示。

## 自动化工业检测（检测制造缺陷）

GPT Vision 可以充当质量检查助手。下一段代码使用多模态 GPT 模型作为自动化质量检查员，分析胶囊图像的*制造缺陷*，并返回结构化的 JSON 检测报告，其中包含检测到的缺陷、位置、严重程度、置信度分数和总体质量评估。

```python
with open("images/capsules.png", "rb") as f:
    image_b64 = base64.b64encode(f.read()).decode()

prompt = """You are an industrial quality-control inspector. Analyze the image carefully. Look for:
Scratches, Cracks, Corrosion, Missing parts, Deformation, Surface
contamination, Paint defects,
Manufacturing anomalies. Return ONLY valid JSON:
{"defect_found": true,  "defects": [{"type": "",  "location": "", "severity": "",
"confidence": 0, "description": ""}],  "overall_quality": "",  "inspection_summary": ""}"""

response = client.responses.create(model="gpt-4o", input=[{"role": "user",
    "content": [{"type": "input_text", "text": prompt},
                {"type": "input_image",
                 "image_url": f"data:image/jpeg;base64,{image_b64}"}]}])
result = response.output_text
print(result)
```

函数 `base64.b64encode(...)` 将图像转换为适合 API 传输的文本格式。

`prompt` 定义了检测任务并指定了所需的 JSON 输出格式。函数 `client.responses.create(...)` 将图像和检查指令发送给 GPT 视觉模型。此处，`"role": "user"` 再次表明提示词和图像是用户输入给模型的内容，`"type": "input_text"` 提供缺陷检查指令，`"type": "input_image"` 提供待分析的胶囊图像，其中 `response.output_text` 以 JSON 格式获取模型的检测报告，`print(result)` 显示生成的质量控制报告。

结果以结构化 JSON 形式返回。

```json
{
    "defect_found": true,
    "defects": [{"type": "crack", "location": "first capsule", "severity": "high", "confidence": 0.95,
        "description": "Visible crack running down the length of the capsule."}, ...]
}
```

随后的可视化代码（参见 GitHub）将 JSON 输出转换为仪表板，显示缺陷位置、严重程度、置信度分数和检测摘要。

