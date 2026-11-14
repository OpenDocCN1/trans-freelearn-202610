
# DPO 之前：预训练

在 DPO 之前，网络会经过预训练以模仿合成动作数据，从而提供强有力的初始化。我们将使用监督学习训练帧编码器和策略网络，以预测每一帧的正确转向动作。这为我们提供了：

* 用于 DPO 训练的**参考策略**——一个强基线。
* 理解基本驾驶模式的预训练网络。

## 行为克隆预训练

以下代码片段展示了基于行为克隆的初始化预训练。

```
# Train encoder + policy_ref on frame->action supervised data (behavior cloning)
bc_epochs = 3
bc_batch = 64
dataset = torch.utils.data.TensorDataset(frames_all, actions_all)
loader = DataLoader(dataset, batch_size=bc_batch, shuffle=True)
opt = optim.Adam(list(frame_encoder.parameters()) + list(policy_ref.parameters()), lr=3e-4)
for ep in range(bc_epochs):
    total_loss = 0.0
    for xb, yb in loader:
        xb = xb.to(DEVICE); yb = yb.to(DEVICE)
        emb = frame_encoder(xb)
        logits = policy_ref(emb)
        loss = F.cross_entropy(logits, yb)
        opt.zero_grad(); loss.backward(); opt.step()
        total_loss += loss.item() * xb.size(0)
    print(f"BC epoch {ep+1}/{bc_epochs} loss: {total_loss/len(dataset):.4f}")
# Save a copy of reference policy logits function (we'll use it for KL)
policy_ref.eval()
frame_encoder.eval()
```

在实践中，您将拥有更复杂的训练流程，并可能使用现有的网络架构和预训练模型。这里我们训练是为了捕捉一些轨迹模式。您可以更改训练轮数和批次大小以延长训练时间。输出应显示收敛趋势，类似于以下行为克隆训练的示例输出：

```
BC epoch 1/3 loss: 1.0846
BC epoch 2/3 loss: 1.0531
BC epoch 3/3 loss: 0.9862
SmallFrameEncoder(
  (net): Sequential(
    (0): Conv2d(3, 16, kernel_size=(5, 5), stride=(2, 2), padding=(2, 2))
    (1): ReLU()
    (2): Conv2d(16, 32, kernel_size=(5, 5), stride=(2, 2), padding=(2, 2))
    (3): ReLU()
    (4): AdaptiveAvgPool2d(output_size=(4, 4))
    (5): Flatten(start_dim=1, end_dim=-1)
    (6): Linear(in_features=512, out_features=128, bias=True)
    (7): ReLU()
  )
)
```

一旦网络使用基本驾驶模式预训练完成，并且您观察到一定程度的收敛，我们就进入下一步。

# 直接偏好优化 (DPO) 训练

**直接偏好优化 (DPO)** 使用带有偏好标签的轨迹对来微调策略。与依赖显式奖励模型的 RLHF 方法不同，DPO 直接优化策略以使其与人类偏好对齐。我们在*第 10 章*中深入介绍了这一点。这里，我们将其应用于运动控制应用。

我们设置中的一些重要实体如下：

* **参考策略**：**行为克隆 (BC)** 模型，作为上一节讨论的基线。
* **偏好来源**：基于平滑度分数的轨迹比较。
* **损失函数**：偏好一条轨迹胜过另一条轨迹的概率上的二元交叉熵。
* **正则化**：KL 惩罚，防止微调后的策略偏离参考策略过远。

训练过程遵循以下步骤：

1. 从数据集中采样轨迹对。
2. 使用偏好函数为每条轨迹评分。
3. 优化策略以增加偏好轨迹的可能性。
4. 使用朝向参考策略的 KL 项进行正则化。

我们现在演示实现过程，将其分解为关键阶段。

## 步骤 1 — 初始化策略和超参数

我们从参考策略（基于 BC 的模型）的副本开始，并准备优化器和 DPO 超参数。

```
policy_theta = SimplePolicy(emb_dim=128, num_actions=3).to(DEVICE)
policy_theta.load_state_dict(policy_ref.state_dict())
dpo_epochs = 6
dpo_lr = 1e-3
kl_coef = 0.02 # KL penalty towards reference
temp = 1.0 # temperature scaling
opt_theta = optim.Adam(policy_theta.parameters(), lr=dpo_lr)
```

### 冻结参考策略

为了在微调期间保持参考策略固定，我们将 `requires_grad` 设置为 `False`。

```
for param in policy_ref.parameters():
    param.requires_grad = False
```

接下来，我们定义 DPO 训练中使用的两个辅助函数。

## 步骤 2 — 工具函数

计算给定策略下的轨迹对数概率，以及可训练策略与参考策略之间的 KL 散度。

```
def batch_traj_logprobs(policy, frames, actions):
    B,T,C,H,W = frames.shape
    emb = frame_encoder(frames.view(B*T,C,H,W).to(DEVICE))
    logits = policy(emb).view(B, T, -1)
    logp = F.log_softmax(logits, dim=-1)
    sel = logp.gather(2, actions.unsqueeze(-1).to(DEVICE)).squeeze(-1)
    return sel.sum(dim=1) # per-trajectory

def batch_kl_policy(p_theta, p_ref, frames):
    B,T,C,H,W = frames.shape
    emb = frame_encoder(frames.view(B*T,C,H,W).to(DEVICE))
    logp_theta = F.log_softmax(p_theta(emb).view(B,T,-1), dim=-1)
    p_ref_probs = F.softmax(p_ref(emb).view(B,T,-1).detach(), dim=-1)
    return F.kl_div(logp_theta, p_ref_probs, reduction='batchmean')
```

## 步骤 3 — 训练循环

每个批次包含成对的轨迹 (A, B) 和一个标签（0 = 偏好 A，1 = 偏好 B）。我们计算 DPO 损失并可选择性地应用 KL 惩罚。

```
for ep in range(dpo_epochs):
    total_loss = 0.0
    for idx in torch.randperm(len(pairs)).split(BATCH):
        batch = [(pairs[i], labels[i]) for i in idx]
      
        def prep(frames_actions):
            frames, acts = zip(*frames_actions)
            f = torch.stack([torch.tensor(fr/255.0).permute(0,3,1,2) for fr in frames])
            a = torch.stack([torch.tensor(ac, dtype=torch.long) for ac in acts])
            return f.to(DEVICE), a.to(DEVICE)
        A_frames, A_actions = prep([clips[ia] for (ia, ib), _ in batch])
        B_frames, B_actions = prep([clips[ib] for (ia, ib), _ in batch])
        logp_A = batch_traj_logprobs(policy_theta, A_frames, A_actions)
        logp_B = batch_traj_logprobs(policy_theta, B_frames, B_actions)
        logp_A_ref = batch_traj_logprobs(policy_ref, A_frames, A_actions)
        logp_B_ref = batch_traj_logprobs(policy_ref, B_frames, B_actions)
        targets = torch.tensor([1.0 if lab==0 else 0.0 for _, lab in batch], device=DEVICE)
        logits = ((logp_A - logp_A_ref) - (logp_B - logp_B_ref)) / temp
        loss_pref = F.binary_cross_entropy_with_logits(logits, targets)
        if kl_coef > 0:
            loss = loss_pref + kl_coef * 0.5 * (
                batch_kl_policy(policy_theta, policy_ref, A_frames) +
                batch_kl_policy(policy_theta, policy_ref, B_frames)
            )
        else:
            loss = loss_pref
        opt_theta.zero_grad()
        loss.backward()
        opt_theta.step()
        total_loss += loss.item() * len(batch)
    print(f"DPO Epoch {ep+1}/{dpo_epochs}, loss {total_loss/len(pairs):.4f}")
```

# 评估与结果

模型完成训练循环并收敛后，可以保存并进行评估。

## 评估微调后的策略

DPO 训练后，可以从**定量**（损失、偏好准确率）和**定性**（轨迹的视觉平滑度）两方面评估策略。视觉检查对于确认学习到的行为反映了预期的特质（如平滑度），而非过拟合训练数据中的伪影，特别有用。*图 12.3* 展示了根据我们的平滑度指标排名**靠前**和**靠后**的轨迹，以及元数据。

![图 12.3 变道行为快照：代表车辆的红点标记显示了居中与激进行为，以及顶部轨迹平滑移动与底部轨迹对比的平滑度分数。（您也可以通过运行笔记本并观察运动轨迹来实现这一点。）](media/file45.png)
