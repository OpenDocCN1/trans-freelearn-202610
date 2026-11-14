

本节详细介绍了一个实用的、动手演示，它在一个相关的机器人场景中生动地说明了 RLHF 的原理。自动驾驶因其直观性、广泛认知度，以及存在难以用传统奖励量化的主观人类偏好（如舒适性、平滑性、社会接受度），而成为理想的说明性示例。为了使这些想法具体化，我们提供了一个使用 Jupyter Notebook 的动手示例，请参阅补充的 GitHub 仓库：`https://github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback/blob/main/chapter12/CARLA_trajectory_tuning.ipynb`。该 Notebook 逐步演示了在模拟驾驶环境中将 DPO 应用于轨迹优化的完整流程。为了保持对概念的关注并降低安装复杂度，我们使用了一个更简单的变道场景。我们不会直接使用模拟环境，但以此示例为起点，你可以将其适配到其他模拟环境，例如 `CARLA`，即开源的 `Car Learning to Act` 模拟器的项目名称。接下来，我们将重点介绍并讨论关键步骤，并为清晰起见包含了选定的代码和输出。

## 概述与动机

我们首先阐述使用 DPO 进行轨迹对齐的动机，并概述流程。有关 DPO 的完整介绍和通用流程，请参阅第 10 章。对于轨迹平滑，流程包含以下步骤。

1.  生成具有不同行为模式（平滑、抖动、激进、居中）的合成驾驶轨迹
2.  基于轨迹平滑度和车道居中创建偏好对
3.  使用 DPO 训练神经网络策略，以学习人类对平滑、居中驾驶的偏好
4.  在轨迹质量指标上评估学习到的策略

## 合成数据生成

为了支持快速实验，Notebook 生成模仿 `CARLA` 前置摄像头视角的合成驾驶场景。这包括不同的驾驶行为（平滑、抖动、激进、居中），并提供了帧渲染和轨迹生成的代码。帧渲染是一个简单的合成道路帧：道路、车道分隔线，以及一辆由 `x_offset` 和 `y_offset` 偏移中心的车。

```
def render_synthetic_frame(x_offset, y_offset, speed=1.0, img_h=IMG_H, img_w=IMG_W):
    img = Image.new("RGB", (img_w, img_h), (100, 120, 120))
    draw = ImageDraw.Draw(img)
    # road rectangle
    draw.rectangle([int(img_w*0.1), 0, int(img_w*0.9), img_h], fill=(40,40,40))
    # lane center line (dashed)
    for i in range(0, img_h, 8):
        draw.rectangle([img_w//2-2, i, img_w//2+2, i+4], fill=(200,200,200))
    # add roadside color
    draw.rectangle([0,0,int(img_w*0.1),img_h], fill=(30,100,30))
    draw.rectangle([int(img_w*0.9),0,img_w,img_h], fill=(30,100,30))
    # draw ego-car marker near bottom, offset by x_offset,y_offset
    cx = img_w//2 + int(x_offset)
    cy = int(img_h*0.8 + y_offset)
    draw.ellipse([cx-5, cy-5, cx+5, cy+5], fill=(255,60,60))
    # Add a debug print to verify x_offset is actually changing
    if abs(x_offset) > 0.1:
        print(f"Car at x_offset={x_offset:.1f}, cx={cx}")
    return np.array(img)

def gen_synth_traj(kind="smooth", traj_len=TRAJ_LEN):
    x = 0.0
    y = 0.0
    frames = []
    actions = []
    for t in range(traj_len):
        if kind == "smooth":
            dx = np.clip(np.random.randn()*0.8, -2,2)  # gentle drift
        elif kind == "jittery":
            dx = np.random.randint(-25, 26)  # large jumps
        elif kind == "aggressive":
            dx = np.random.choice([-8, 8])
        elif kind == "centered":
            dx = int((0 - x)*0.2) + np.random.randint(-1,2)
        else:
            dx = np.random.randint(-4, 5)
        x = float(x + dx)
        action = 0 if dx < -0.5 else 2 if dx > 0.5 else 1
        actions.append(int(action))
        frames.append(render_synthetic_frame(x_offset=x, y_offset=y))
    return frames, np.array(actions, dtype=np.int64)
```

## 可视化轨迹

可视化对于理解智能体行为至关重要。Notebook 提供了将生成的轨迹显示为 GIF 的工具，使不同驾驶风格的效果立竿见影。让我们生成并可视化不同驾驶行为的示例，以了解我们的合成数据。下面的函数 `show_gif_inline` 将帮助可视化由 `gen_synth_traj` 函数生成的轨迹。它将内存中的 RGB 帧列表作为内联 GIF 显示。

```
def show_gif_inline(frames, fps=6):
    # Ensure uint8 frames
    frames_uint8 = [(f if f.dtype == np.uint8 else (f*255).astype(np.uint8)) for f in frames]
    gif_bytes = imageio.mimsave('<bytes>', frames_uint8, fps=fps, format='GIF', loop=0, return_bytes=True)
    display(IPyImage(data=gif_bytes))
```

你可以通过在 Notebook 单元格中调用这些工具函数来生成并显示一条平滑轨迹。

```
frames_smooth, _ = gen_synth_traj(kind="smooth")
show_gif_inline(frames_smooth)
```

## 偏好评分

基于偏好的 RL 中的关键步骤是定义什么使一条轨迹比另一条更优。在这里，评分函数结合了平滑度和车道居中度，使用鲁棒的红色像素检测来定位车辆标记。

### 偏好评分：定义“良好”驾驶

我们需要定义什么使一条轨迹优于另一条。我们的评分函数结合了：

*   **平滑度评分（70% 权重）**，衡量车辆横向位置在各帧间的一致性，并惩罚逐帧的大幅移动。平滑度评分越高意味着驾驶越平滑。
*   **居中度评分（30% 权重）** 衡量车辆保持在车道中心的程度，并惩罚偏离道路中线的行为。评分越高意味着车道保持越好。

这些是从自动驾驶角度出发的现实指标。对于本用例的范围，我们将仅使用这些指标，并应用**红色像素检测**来可靠地找到车辆标记。颜色像素检测比基于亮度的检测更鲁棒，后者可能会将车道线与车辆混淆。

## 神经网络架构

我们使用一个轻量级 CNN 作为帧编码器和一个简单的策略网络，首先通过行为克隆（监督学习）训练，然后用 DPO 微调。我们的系统包含一个帧编码器 `frame_encoder`，一个将 64×64 RGB 帧转换为特征嵌入的小型 CNN，以及一个将这些嵌入映射为动作概率（左/直/右）的策略网络 `policy_ref`。

```
class SmallFrameEncoder(nn.Module):
    def __init__(self, out_dim=64):
        super().__init__()
        self.net = nn.Sequential(
            nn.Conv2d(3, 16, 5, stride=2, padding=2), nn.ReLU(),
            nn.Conv2d(16, 32, 5, stride=2, padding=2), nn.ReLU(),
            nn.AdaptiveAvgPool2d((4,4)),
            nn.Flatten(),
            nn.Linear(32*4*4, out_dim), nn.ReLU()
        )
    def forward(self, x):
        return self.net(x)

frame_encoder = SmallFrameEncoder(out_dim=128).to(DEVICE)

# Policy model (reference policy)
class SimplePolicy(nn.Module):
    def __init__(self, emb_dim=128, num_actions=3):
        super().__init__()
        self.model = nn.Sequential(
            nn.Linear(emb_dim, 64), nn.ReLU(),
            nn.Linear(64, num_actions)
        )
    def forward(self, emb):
        return self.model(emb)  # logits

policy_ref = SimplePolicy(emb_dim=128, num_actions=3).to(DEVICE)
```

确定网络架构后，我们将首先在合成动作数据上使用行为克隆训练这些网络。

## 行为克隆（监督预训练）

