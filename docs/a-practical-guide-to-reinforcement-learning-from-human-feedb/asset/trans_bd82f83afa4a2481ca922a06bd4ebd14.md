

## 表 12.1：基于关键词效果的模拟人类审美评分

| 关键词      | 评分 | 描述                      |
|-------------|------|---------------------------|
| minimalist  | +2   | 简洁、简单、整洁          |
| cozy        | +2   | 令人向往、温暖、舒适      |
| cluttered   | –3   | 杂乱、令人不知所措        |
| warm        | +1   | 令人感到欢迎的氛围        |
| luxurious   | +1   | 高级、优雅的感觉          |

为了模拟人类的审美判断，我们定义了一个简单的评分函数，根据关键词分配分数：

```python
def interpretable_aesthetic_score(prompt):
    score = 0
    explanation = []
    if "minimalist" in prompt:
        score += 2
        explanation.append("minimalist: +2 (clean, simple)")
    if "cozy" in prompt:
        score += 2
        explanation.append("cozy: +2 (inviting)")
    if "cluttered" in prompt:
        score -= 3
        explanation.append("cluttered: –3 (messy)")
    if "warm" in prompt:
        score += 1
        explanation.append("warm: +1 (welcoming)")
    if "luxurious" in prompt:
        score += 1
        explanation.append("luxurious: +1 (premium)")
    return score, explanation
```

## 创建用于训练的成对偏好数据集

正如前几章关于 RLHF 和 DPO 所述，区分好坏示例的偏好对在此也很有用。我们使用评分函数来模拟图像对之间的人类偏好：

```python
pairs = []
num_pairs = 100
for _ in range(num_pairs):
    idx_a, idx_b = random.sample(range(len(prompts)), 2)
    score_a, explanation_a = interpretable_aesthetic_score(prompts[idx_a])
    score_b, explanation_b = interpretable_aesthetic_score(prompts[idx_b])
    preferred = idx_a if score_a >= score_b else idx_b
    if random.random() < 0.05:
        print(f"\nPrompt A: {prompts[idx_a]}\nScore: {score_a}\n{', '.join(explanation_a)}")
        print(f"Prompt B: {prompts[idx_b]}\nScore: {score_b}\n{', '.join(explanation_b)}")
        print(f"Preferred: {'A' if preferred == idx_a else 'B'}")
    pairs.append((idx_a, idx_b, preferred))
```

这些成对数据代表用于训练奖励模型的偏好标签。随后我们定义了一个自定义的 PyTorch 数据集类，用于加载图像对及其偏好标签：

```python
transform = transforms.Compose([
    transforms.Resize((224, 224)),
    transforms.ToTensor(),
])

class PreferenceDataset(Dataset):
    def __init__(self, pairs, prompts, transform):
        self.pairs = pairs
        self.prompts = prompts
        self.transform = transform

    def __len__(self):
        return len(self.pairs)

    def __getitem__(self, idx):
        idx_a, idx_b, preferred_idx = self.pairs[idx]
        img_a = Image.open(f"images/image_{idx_a}.png").convert("RGB")
        img_b = Image.open(f"images/image_{idx_b}.png").convert("RGB")
        img_a = self.transform(img_a)
        img_b = self.transform(img_b)
        label = 1 if preferred_idx == idx_a else 0
        return img_a, img_b, label
```

偏好对、提示词和变换可以轻松加载到数据集中，`torch` dataloader 允许按批次加载它们。

```python
dataset = PreferenceDataset(pairs, prompts, transform)
dataloader = DataLoader(dataset, batch_size=4, shuffle=True)
```

## 奖励模型设置与架构

对于奖励模型的设置和训练架构，我们首先利用预训练的 `ResNet-18` 作为图像编码器。通过将其最后的全连接分类层替换为恒等函数，我们保留了其倒数第二层丰富的 512 维特征表示。

```python
feature_extractor = models.resnet18(pretrained=True)
feature_extractor.fc = nn.Identity()
```

与传统的特征提取不同，我们允许对完整模型进行微调 —— 使我们的奖励模型能更直接地适应审美信号，甚至包括更早期的卷积层。

```python
for param in feature_extractor.parameters():
    param.requires_grad = True
```

接下来，我们定义一个小型的 `reward head`（奖励头），将 512 维的图像嵌入映射为单一的标量奖励：

```python
reward_head = nn.Sequential(
    nn.Linear(512, 128),
    nn.ReLU(),
    nn.Linear(128, 1)
)
```

完整的奖励模型可以如 `RewardModel` 类所示实例化。我们随后堆叠一个小型 MLP 以输出标量奖励。

```python
class RewardModel(nn.Module):
    def __init__(self, backbone, head):
        super().__init__()
        self.backbone = backbone
        self.head = head

    def forward(self, x):
        features = self.backbone(x)
        return self.head(features)

reward_model = RewardModel(feature_extractor, reward_head).cuda()
```

## 从成对偏好中学习

奖励模型使用成对偏好损失进行训练。对于每一对带标签的图像（A 优于 B 或反之），我们计算它们各自的分数，如果不那么受偏好的图像获得更高分数，则惩罚模型。

```python
optimizer = optim.Adam(reward_model.parameters(), lr=1e-4)
epochs = 10
for epoch in range(epochs):
    running_loss = 0.0
    for img_a, img_b, label in dataloader:
        img_a = img_a.cuda()
        img_b = img_b.cuda()
        label = label.cuda().float()
        score_a = reward_model(img_a).squeeze()
        score_b = reward_model(img_b).squeeze()
        pref_diff = score_a - score_b
        loss = torch.mean(torch.relu(1.0 - (pref_diff * (2 * label - 1))))
        optimizer.zero_grad()
        loss.backward()
        optimizer.step()
        running_loss += loss.item()
    print(f"Epoch [{epoch+1}/{epochs}], Loss: {running_loss/len(dataloader):.4f}")
```

这种公式使奖励模型能够逼近人类标注者隐式使用的底层审美函数。一经训练，奖励模型即可评估未见图像的审美质量。

```python
for i in range(3):
    img = Image.open(f"images/image_{i}.png").convert("RGB")
    img_tensor = transform(img).unsqueeze(0).cuda()
    score = reward_model(img_tensor).item()
    print(f"Image {i} score: {score:.2f} — Prompt: {prompts[i]}")
    display(img)
```

较高的分数对应更符合学习到的审美偏好的图像，反映了模型内化的奖励函数。`图 12.2` 展示了基于训练奖励模型评分的示例：

![图 12.2：基于奖励模型评分的生成房间图像。左侧展示豪华客厅，右侧展示乡村风格卧室。](media/file44.png)

**图 12.2：** 基于奖励模型评分的生成房间图像。左侧展示豪华客厅，右侧展示乡村风格卧室。

这种体现了可解释性和模块化。一旦我们拥有了基于偏好训练的奖励函数，就可以将其接入下游图像生成管线 —— 用于对输出进行排序、筛选结果，甚至利用 RL 或优化算法引导生成器的潜在空间遍历。

## 超越静态图像

我们讨论的示例演示了人类偏好如何训练奖励模型来评估图像美学并相应地引导生成。虽然我们专注于静态视觉输出，但这些原则可推广到单张图像之外。反馈引导的生成自然地扩展到动态领域。例如，偏好模型可以对整个输出序列进行排序或优化 —— 使其适用于视频生成，其中时间连贯性和视觉吸引力随时间演变；或适用于机器人运动，其中平滑度和意图一致性贯穿动作序列。在下一节中，我们将探讨偏好建模和学习到的奖励函数如何支持具身智能体中的运动生成与对齐 —— 将 RLHF 从视觉领域推向运动、行为与控制的领域。

## 教会机器运动

