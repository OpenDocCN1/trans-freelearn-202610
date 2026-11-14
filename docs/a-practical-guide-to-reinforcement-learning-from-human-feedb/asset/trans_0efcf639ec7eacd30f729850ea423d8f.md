
# 具有可解释偏好的美学图像生成

正如本书中所讨论的，`RLHF` 和其他人类反馈系统使我们能够将语言模型与人类偏好对齐。但这些对齐技术在视觉领域同样强大。图像生成模型，如基于扩散的系统和大规模 Transformer，能够生成极其多样的视觉内容。然而，“质量”和“美学”具有高度的主观性和上下文依赖性。`RLHF` 通过显式评分或成对比较，为捕捉和建模人类视觉偏好提供了一个系统框架，并用于优化图像生成策略以反映这些偏好。在实际实现中，这通常涉及使用基于人类或模拟偏好数据训练的奖励模型，对基于扩散或 Transformer 的图像生成器进行微调。在本节中，我们通过一个美学图像生成的详细示例来演示这一点，展示模拟人类评分如何引导模型生成更令人愉悦或更符合品牌调性的视觉输出。

正如我们在前几章所见，`RLHF` 极大地提升了文本生成模型与人类期望的对齐程度。视觉生成领域正在发生类似的革命。生成式图像模型如 `Stable Diffusion`、`DALL·E` 和 `Midjourney` 能够生成几乎任何风格的图像，但什么构成“好”或“美”的图像却是深度主观且依赖语境的。将美学或风格偏好显式编码进奖励函数几乎是不可能的，但人类判断可以填补这一空白。

现在让我们从概念概述转向实际演示。我们通过一个具体的工作示例来演示这一点：**具有可解释偏好的美学图像生成**。在该示例中，“极简”、“舒适”或“温暖”等偏好被转化为可解释的评分函数，我们将展示构建、训练和应用奖励模型以引导图像输出的每一步。

读者关键点：

*   理解图像的成对或比较偏好数据。
*   了解如何训练捕捉主观判断的奖励模型。
*   学习如何将人类反馈集成到图像生成流程中。

除了静态图像，这些原则还可扩展到视频生成和场景编辑，其中持续反馈可以以更丰富的方式引导风格、叙事流程或构图。

## 视觉偏好对齐简介

要开始实现上述美学图像对齐示例，我们首先需要了解如何以视觉形式表达人类偏好。在语言任务中，我们通常依赖人工标注的偏好或成对比较来引导模型输出。同样的思路也适用于图像——如果我们能模拟或收集关于视觉特征的人类偏好，就可以学习一个奖励模型来表示这些判断，并训练模型针对它们进行优化。这一模拟标注步骤与前文讨论的文本响应偏好数据收集过程平行，只是应用于视觉属性。

在本示例中，我们定义了可解释的美学维度，例如：

*   **色彩温度**（暖色调 vs. 冷色调）
*   **视觉简洁度**（极简 vs. 杂乱）
*   **亮度与曝光**

这些视觉特征易于向人类解释，且可使用简单的图像特征进行测量。通过基于这些特征创建模拟标签，我们生成成对偏好数据，用于训练奖励模型。我们现在将逐步演示完整流程。（完整的导入和数据加载代码见配套的 Colab 笔记本。此处仅展示概念核心以保持清晰。）

## 环境准备：安装依赖与加载模型

要在 Colab 环境或全新环境中运行此示例，请先安装所需库：

```
!pip install diffusers transformers accelerate
!pip install torch torchvision
!pip install matplotlib pillow tqdm
```

安装并导入后，我们加载预训练的 `Stable Diffusion` 模型作为图像生成器：

```
pipe = StableDiffusionPipeline.from_pretrained("CompVis/stable-diffusion-v1-4")
pipe = pipe.to("cuda")
```

## 生成提示词图像

我们首先为客厅内部设计一小组风格丰富的提示词：

```
prompts = [
    "A cozy minimalist living room",
    "A cluttered bohemian living room",
    "A modern luxurious living room",
    "A warm rustic living room"
]
```

每个提示词传递给扩散管道以生成并可视化图像：

```
images = []
for p in prompts:
    img = pipe(p).images[0]
    img.save(f"{p.replace(' ', '_')}.png")
    display(img)
    images.append(img)
```

为丰富数据集，我们通过混合风格和情感形容词合成 50 个额外提示词：

```
styles = ["minimalist", "modern", "rustic", "bohemian", "luxurious", "industrial"]
feelings = ["cozy", "spacious", "bright", "warm", "inviting", "airy"]
elements = ["living room", "bedroom", "office", "kitchen"]
prompts = []
for _ in range(50):
    style = random.choice(styles)
    feeling = random.choice(feelings)
    element = random.choice(elements)
    prompt = f"A {feeling} {style} {element} interior design, natural light"
    prompts.append(prompt)
```

然后为每个提示词生成图像并保存，如下所示：

```
os.makedirs("images", exist_ok=True)
for i, p in enumerate(tqdm(prompts)):
    img = pipe(p).images[0]
    img_path = f"images/image_{i}.png"
    img.save(img_path)
    if i < 3:  # 显示前几个示例
        print(f"\nPrompt: {p}")
        display(img)
```

![图 12.1 使用 Stable Diffusion 模型和生成提示词生成的图像示例。](media/file43.png)

**图 12.1** 使用 `Stable Diffusion` 模型和生成提示词生成的图像示例。

在图 12.1 中，你可以看到一个生成的图像示例及其提示词。在笔记本中，我们显示一小部分图像以检查生成过程。生成图像后，我们将接下来模拟人类偏好以对其进行标注。

## 使用可解释评分函数模拟偏好

为以可复现的方式引入人类反馈，我们定义一个简单的**可解释偏好函数**来模拟“人类美学评分”。该评分基于常见设计原则分配，详见*表 12.1*。在真实的对齐流水线中，这些评分将来自实际人工评估；此处我们使用透明的基于规则的函数来复现人类偏好数据的结构。

| **关键词** | **评分影响** | **解释** |
| --- | --- | --- |

