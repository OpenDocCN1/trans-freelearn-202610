
图 12.3 变道行为快照：代表车辆的红点标记显示居中与激进行为，以及顶部轨迹与底部轨迹平滑移动程度的平滑度评分。（您也可以通过运行笔记本并查看运动轨迹来实现这一点。）

动画 GIF 使得轻松发现运动平滑度的定性差异成为可能。这些可视化包含在本节开头引用的 GitHub 笔记本中。首先，我们通过保存顶部和底部 GIF 来捕获一些运动轨迹。为了更好地可视化并查看与标签和偏好评分的相关性，我们添加了带标题的 GIF 并将其显示。相关代码可在以下链接获取：[https://github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback/blob/main/chapter12/CARLA_trajectory_tuning.ipynb](https://github.com/PacktPublishing/A-Practical-Guide-to-Reinforcement-Learning-from-Human-Feedback/blob/main/chapter12/CARLA_trajectory_tuning.ipynb)，并展示了那些选定的动画 GIF。对于定量评估，使用由 `policy_pref_score` 函数计算的策略偏好评分，我们可以评估策略偏好哪条轨迹，并与真实偏好标签进行对比。

```
def policy_pref_score(policy, frames):
    # log-prob sequence for actions for visual-only clips
    # argmax of policy probabilities at each frame
    t = torch.tensor(
        np.transpose(np.stack(frames).astype(np.float32) / 255.0, (0, 3, 1, 2))
    ).unsqueeze(0).to(DEVICE)  # (1,T,C,H,W)
    with torch.no_grad():
        B, T, C, H, W = t.shape
        frames_ = t.view(B * T, C, H, W)
        emb = frame_encoder(frames_)
        logits = policy(emb).view(B, T, -1)
        probs = F.softmax(logits, dim=-1)
        # compute sum of max action log-probs as proxy score
        maxp, _ = probs.max(dim=2)  # (B,T)
        score = torch.log(maxp + 1e-9).sum().item()
    return score
```

可以通过与真实标签进行比较，计算样本轨迹及其相对于真实情况的偏好匹配准确率。

```
# Evaluate on some sample pairs
n_eval = min(40, len(pairs))
correct = 0
for idx in range(n_eval):
    ia, ib = pairs[idx]
    label = labels[idx]
    fa, aa = clips[ia]
    fb, ab = clips[ib]
    # If action logs, use sequence logprob else use proxy
    if aa is not None and ab is not None:
        sA = trajectory_logprob(policy_theta, fa, aa)
        sB = trajectory_logprob(policy_theta, fb, ab)
        preferA = (sA.item() > sB.item())
    else:
        sA = policy_pref_score(policy_theta, fa)
        sB = policy_pref_score(policy_theta, fb)
        preferA = (sA > sB)
    truthA = (label == 0)
    if preferA == truthA:
        correct += 1
acc = correct / n_eval
print(f"Preference-match accuracy over {n_eval} pairs: {acc:.3f}")
```

`n_eval` 对的偏好匹配准确率可能还不够，但可以改进。接下来，我们列出可能的改进方法。

## 实验与改进

为了提高偏好匹配准确率，尤其是在实践中，请考虑以下策略。

*   **增加训练数据**：生成更多合成轨迹和偏好对。更多样化和更多数量的示例有助于策略更好地泛化到偏好函数。
*   **增强偏好评分**：优化 `traj_smoothness_score` 以更好地捕捉类人偏好。考虑添加更细微的特征（例如，惩罚偏离车道、奖励平滑修正，或包含速度一致性）。
*   **调整 DPO 超参数**：尝试不同的 DPO 训练参数。
    *   增加 DPO 的训练轮数。
    *   调整 KL 惩罚项（`kl_coef`）以平衡模仿学习和偏好学习。
    *   在 DPO 损失中尝试不同的温度（`temp`）以获得更清晰的偏好区分。
    *   使用更低的学习率以实现更稳定的收敛。
*   **提升模型容量**：为帧编码器使用更深或更宽的 `CNN`，或尝试预训练模型（如 `ResNet18`）以获得更好的视觉特征提取。增加策略网络的规模。
*   **数据增强**
    *   对帧应用随机变换（如亮度、对比度、微小平移）以使策略更鲁棒。
*   **课程学习**：从简单的偏好对（分差大）开始训练，然后逐渐引入困难的偏好对（差异细微）。
*   **更好的动作监督**：如果可能，为行为克隆使用更准确或更多样的动作标签，以提供更强的初始化。
*   **正则化**：添加 Dropout 或权重衰减以防止过拟合，特别是在数据集较小时。
*   **评估与调试**：可视化策略与偏好函数不一致的失败案例，以识别系统性错误（例如在特定视觉场景下的混淆）。

通过迭代应用这些改进并监控偏好匹配准确率，您应该能看到策略与偏好函数之间对齐程度的可衡量提升。请注意，这些做法大多与监督学习的实践相似。

## 更广泛的意义与未来方向

在见证了偏好引导的微调如何改进驾驶轨迹后，我们可以将这些原则推广到其他具身系统。自动驾驶中演示的原则适用于广泛的物理 AI 系统，包括无人机飞行路径优化（用于配送或摄影的平滑度）、仓储物流（优化人机共享空间中机器人路径的效率与安全）、手术机器人（提升精度与柔和度）以及辅助机器人（符合个体用户的舒适度与偏好）。

*   **与其他对齐技术的融合**：RLHF 正日益与可解释性、安全约束、因果推理等其他方法相结合，以构建更稳健、更可信、更以人为中心的自主系统。
*   **RLHF 作为对齐物理 AI 的基石**：RLHF 对于弥合客观任务完成与真实机器人任务中人类期望的主观、定性细微差别之间的鸿沟至关重要。
*   **从“能不能做？”到“做得好不好、合不合心意？”**：该框架将关注点从单纯的功能性能力转移到确保自主系统以安全、舒适、高效的方式执行动作，并最终与人类意图和价值观保持一致。

这个简化的自动驾驶示例展示了如何利用对好坏轨迹的人类反馈进行对齐，从而帮助改进汽车的运动轨迹，变道或车辆在交通中行驶及其相对于车道的行为只是众多行为场景中的一个。在下一节中，我们将讨论如何改进另一种模态：音频生成。

# 协奏声音：音频与音乐生成

虽然 RLHF 已彻底改变了基于文本的 AI 对齐，但其原理强有力地延伸到了其他模态——最著名的是音频和音乐生成。在本节中，我们将探讨人类反馈如何引导生成模型产生更符合人类偏好的声音和音乐。

## 基于人类反馈的音频生成

为了演示音频领域的 RLHF，我们利用 Hugging Face 的 `MusicGen` 模型进行文本到音频的生成，并配合 `librosa` 和 `soundfile` 进行特征提取。请确保已安装 `transformers`>=4.36 和 `librosa`；这些示例假设为 `MusicGen` 提供 GPU 运行时。这使我们能够从文本提示生成音频片段并分析其属性：
