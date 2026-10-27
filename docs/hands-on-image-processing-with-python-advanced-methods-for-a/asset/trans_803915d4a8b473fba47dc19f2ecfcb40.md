

## 图像修复

修复通过利用周围像素的上下文信息来填补缺失或不需要的图像区域。

```
pipe = StableDiffusionInpaintPipeline.from_pretrained(
    "runwayml/stable-diffusion-inpainting").to("cuda")
image = Image.open('man.jpg').convert('RGB')
mask = Image.open('man_mask.png').convert('RGB')
result = pipe(prompt="natural background", image=image, mask_image=mask).images[0]
```

掩码标识要修改的区域，而扩散模型生成与周围场景一致的内容。应用包括对象移除、图像修复和内容感知编辑。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_716_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_716_1.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_716_2.png)

图 13.33 – 使用 Stable Diffusion 进行图像修复

## 艺术风格生成

扩散模型仅通过提示工程即可模仿各种艺术风格。

```
from diffusers import StableDiffusionPipeline
pipe = StableDiffusionPipeline.from_pretrained(
    "runwayml/stable-diffusion-v1-5", torch_dtype=torch.float16).to("cuda")
styles = ["oil painting", "anime illustration", "pixel art", "pencil sketch"]
for style in styles:
    prompt = (
        f"A majestic castle on a mountain, "
        f"{style}"
    )
    img = pipe(prompt, num_inference_steps=30, guidance_scale=8).images[0]
```

对于每种风格，提示“A majestic castle on a mountain”都会加上所需的艺术描述进行扩充。生成的图像在保留场景语义的同时采用截然不同的视觉外观，展示了现代扩散模型强大的风格迁移能力。

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_717_0.png)

*图 13.34 – 使用 Stable Diffusion 进行艺术风格生成*

除了基于风格的提示工程之外，扩散模型还可以使用外部视觉和结构条件机制进行引导，以实现更精确和可控的生成，我们将在下一节中探讨。

## IP-Adapter 和 ControlNet 用于身份保持的姿势引导生成

文本到图像扩散模型的一个局限是，它们无法可靠地在同时控制姿势和构图的情况下保持特定人物的外观。两种最新的条件机制解决了这个问题。**IP-Adapter（Image Prompt Adapter）** 将参考图像的视觉信息注入扩散模型，而 **ControlNet** 则施加诸如边缘、深度图、分割掩码或人体姿势等结构约束。通过结合这两种方法，可以生成在遵循所需姿势的同时保持人物身份的图像。

**IP-Adapter：** 传统的 Stable Diffusion 仅依赖文本嵌入。IP-Adapter 通过从参考图像中提取视觉特征并将其通过额外的交叉注意力层注入扩散 U-Net 来扩展条件机制。设 *T* 表示文本嵌入，*I* 表示从视觉编码器提取的图像嵌入。扩散模型变为 ε<sub>θ</sub>(z<sub>t</sub>, t, T, I)，

其中 z_t 是噪声潜在表示。因此，生成过程同时受文本和视觉信息的引导。IP-Adapter 使用以下方式加载到 Stable Diffusion 流水线中：

```
pipe.load_ip_adapter("h94/IP-Adapter", subfolder="models", weight_name="ip-adapter_sd15.bin")
pipe.set_ip_adapter_scale(0.8)
```

参数 ip_adapter_scale 控制参考图像的影响。较大的值会增加身份保持，而较小的值会为文本提示提供更大的自由度。

ControlNet：尽管 IP-Adapter 保留了外观，但它并未明确约束图像几何。ControlNet 引入了一个辅助网络，对扩散过程进行结构信号的条件化。给定控制图像 c，去噪过程变为 ε_θ(z_t, t, T, c)。在此示例中，控制信号是描述所需身体姿势的 OpenPose 骨架：

```
controlnet = ControlNetModel.from_pretrained("lllyasviel/control_v11p_sd15_openpose")
```

OpenPose 图像充当几何蓝图，迫使生成的主题遵循指定的姿势，同时与文本提示保持一致。

## 结合身份和姿势约束：完整的流水线结合了三种条件来源：

- Text prompt – controls semantic content
- Reference image (IP-Adapter) – preserves identity and appearance
- OpenPose skeleton (ControlNet) – constrains body pose

Stable Diffusion 流水线构建为：

```
pipe = StableDiffusionControlNetPipeline.from_pretrained("runwayml/stable-diffusion-v1-5",
    controlnet=controlnet, torch_dtype=torch.float16)
```

然后使用以下方式进行生成：

```
images = pipe(prompt="professional portrait photograph",
    image=openpose_image, ip_adapter_image=reference_image,
    num_inference_steps=30, guidance_scale=7.5, num_images_per_prompt=4).images
```

这里，openpose_image 提供姿势信息，而 reference_image 提供面部外观和身份线索。生成的图像继承了参考图像的面部特征和视觉风格，同时采用 OpenPose 骨架编码的姿势。与通常会改变样本间身份的普通文本到图像生成不同，结合 IP-Adapter 和 ControlNet 的框架能够实现受控合成，其中身份和姿势可以独立操纵。这种能力对于头像生成、虚拟摄影、数字人类、角色动画、时尚可视化和姿势引导人像合成特别有价值。

## IP-Adapter + OpenPose ControlNet

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_719_0.png)

图 13.35 – 使用 ip-adapter 和 controlnet 进行身份保持的姿势引导图像生成

安装阶段加载 Stable Diffusion v1.5、OpenPose ControlNet 模型和 IP-Adapter 权重。流水线随后附加 IP-Adapter 注意力层并使用 set_ip_adapter_scale() 设置其影响。在推理期间，OpenPose 图像通过 image 参数提供，身份参考通过 ip_adapter_image 提供，文本提示指定所需外观。扩散模型融合所有三个条件来源以生成在保持身份的同时符合目标姿势的光栅化逼真人像。

## 使用 DALL·E 2 和 DALL·E 3 API 进行图像生成

OpenAI 的 DALL·E 模型系列能够从自然语言描述生成高质量的文本到图像。给定文本提示 y，模型学习条件分布 p(x|y)，其中 x 表示生成的图像。DALL·E 2 引入了强大的文本引导图像合成和变体生成，而 DALL·E 3 则显著改善了提示理解、组合推理、场景连贯性、排版以及对详细说明的遵循。这些模型广泛用于概念艺术、广告、产品可视化、讲故事、建筑设计和创意内容生成。

要使用 API，请在 OpenAI Platform(https://platform.openai.com/?utm_source=chatgpt.com) 上创建账户，从仪表板生成 API 密钥，添加计费积分，并注意图像生成是按使用量计费的，成本取决于模型（例如 DALL·E 2 与 DALL·E 3）、图像分辨率和生成的图像数量。

以下代码配置 OpenAI API 并安装所需的库：

```
!pip install openai pillow matplotlib requests

import os
os.environ['OPENAI_API_KEY'] = '<your API key here>'

from openai import OpenAI
client = OpenAI()
```

API 密钥对 OpenAI 图像生成服务的请求进行身份验证，而 OpenAI 客户端提供对 DALL·E 图像生成端点的访问。

## 使用 DALL·E 2 生成图像：DALL·E 2 直接从文本提示创建图像。

```
resp = client.images.generate(model="dall-e-2", prompt=prompt,
    size="1024x1024", n=1)
```

模型接收文本描述并生成一个或多个与提示一致的图像。在演示中，对同一提示请求多次生成以可视化输出多样性。由于图像生成具有随机性，反复执行通常会产生对同一描述的不同解释。辅助函数 generate_dalle2() 为每个提示创建基础图像和两个额外变体。

```
def generate_dalle2(prompt):  # DALL-E 2 generator
    resp_base = client.images.generate(model="dall-e-2",
                                    prompt=prompt, size="1024x1024",  n=1)
    base_img = get_image(resp_base.data[0].url)
    resp_vars = client.images.generate(model="dall-e-2",
                                    prompt=prompt, size="1024x1024",  n=2)
    var1 = get_image(resp_vars.data[0].url)
    var2 = get_image(resp_vars.data[1].url)
    return base_img, var1, var2
```

## 使用 DALL·E 3 生成图像：DALL·E 3 的访问方式类似。

```
resp = client.images.generate(model="dall-e-3", prompt=prompt,
size="1024x1024")
```

类似的辅助函数 generate_dalle3(prompt) 生成同一提示的三个独立实现。与 DALL·E 2 相比，DALL·E 3 通常产生更好的提示遵循性、改进的空间推理、更连贯的对象关系、更高的视觉真实感、更优越的排版和文本渲染以及对复杂指令更准确的解释。

## 基于提示的图像生成：实验在多样化的想象提示集合上评估两种模型。

```
prompts = [
    "Photo of Victoria Memorial Hall in the winter...",
```

