

```python
import torch, numpy as np, librosa, soundfile as sf
from transformers import pipeline
from IPython.display import Audio, display
# Load once
generator = pipeline("text-to-audio", model="facebook/musicgen-small")
def generate_audio(prompt: str, seed: int | None = None):
    # MusicGen pipeline doesn't take `seed`; seed the RNG directly.
    if seed is not None:
        torch.manual_seed(seed)
        if torch.cuda.is_available():
            torch.cuda.manual_seed_all(seed)
    out = generator(prompt)  # returns dict or list[dict]
    item = out[0] if isinstance(out, list) else out
    audio = item["audio"]
    sr = item.get("sampling_rate", 32000)
    return audio.astype(np.float32), sr
def extract_features(audio: np.ndarray, sr: int) -> np.ndarray:
    # Simple, transparent proxies for calm vs energetic
    centroid = float(librosa.feature.spectral_centroid(y=audio, sr=sr).mean())
    zcr      = float(librosa.feature.zero_crossing_rate(y=audio).mean())
    return np.array([centroid, zcr], dtype=np.float32)
```

在这里，我们根据提示词生成音频，并提取可解释的特征——频谱质心（亮度）和过零率（能量）——作为“平静”或“充满活力”等音乐特质的代理指标。

### 建模偏好：Bradley-Terry 方法

为了模拟 RLHF，我们收集音频片段之间的成对偏好，并使用 **Bradley-Terry 模型** 来学习最能解释这些偏好的特征权重：

```python
import numpy as np
from scipy.optimize import minimize
# comparisons: list of (i, j, y) where y=1 if i preferred over j, else 0
def bt_negloglik(w: np.ndarray, X: np.ndarray, comparisons: list[tuple[int,int,int]]) -> float:
    scores = X @ w  # latent score per item
    loss = 0.0
    for i, j, y in comparisons:
        si, sj = scores[i], scores[j]
        diff = si - sj
        # P(i > j) = sigmoid(si - sj)
        p = 1 / (1 + np.exp(-diff))
        loss -= y * np.log(p + 1e-9) + (1 - y) * np.log(1 - p + 1e-9)
    return loss
def train_bt(X: np.ndarray, comparisons: list[tuple[int,int,int]]) -> np.ndarray:
    w0 = np.zeros(X.shape[1], dtype=np.float64)
    res = minimize(bt_negloglik, w0, args=(X, comparisons), method="BFGS")
    return res.x  # learned feature weights
def preference_score(features: np.ndarray, w: np.ndarray) -> float:
    return float(features @ w)
```

这种方法使我们能够根据学习到的类人标准对未来的生成结果进行排序，使输出与期望的属性保持一致。

### 候选生成与排序

我们为一个提示词生成多个候选音频片段，提取它们的特征，并模拟成对偏好。然后使用学习到的偏好模型对候选片段进行排序：

```python
# --- Create pipeline once (outside loops) ---
gen_pipe = pipeline("text-to-audio", model="facebook/musicgen-small")
def generate_candidates(prompt, n=3):
    outs = gen_pipe([prompt] * n, forward_params={"do_sample": True, "max_new_tokens": 256})
    clips = []
    for i, out in enumerate(outs):
        audio = out["audio"]
        sr = out["sampling_rate"]
        feats = extract_features(audio, sr)
        clips.append((audio, sr, feats, i))
    return clips
# Example pool
candidates = generate_candidates("a calm piano melody", n=3)
# Build feature matrix for BT training
X = np.array([c[2] for c in candidates])
# Synthetic pairwise prefs: prefer lower centroid
comparisons = []
for i in range(len(candidates)):
    for j in range(i+1, len(candidates)):
        yi = 1 if candidates[i][2][0] < candidates[j][2][0] else 0
        comparisons.append((i, j, yi))
# Train BT weights
w_bt = train_bt(X, comparisons)
print("Learned BT weights:", np.round(w_bt, 4))
# Rank candidates (use learned BT model consistently)
ranked = sorted(
    [(audio, sr, feats, preference_score(feats, w_bt), idx)
     for audio, sr, feats, idx in candidates],
    key=lambda x: -x[3]
)
```

这一过程演示了 RLHF 如何在非语言领域引导生成模型，利用人类反馈来塑造输出。

### 可视化偏好分数

为了直观展示 RLHF 的影响，我们可视化分配给每个候选片段的偏好分数，展示学习到的模型如何区分不同的音频输出：

```python
import matplotlib.pyplot as plt
# --- Generate pre-RLHF candidates ---
pre_clips = generate_candidates("a calm piano melody", n=5)
pre_X = np.array([c[2] for c in pre_clips])
pre_scores = np.array([preference_score(feats, w_bt) for _, _, feats, idx in pre_clips])
# --- Rank post-RLHF candidates (simulated by preference model) ---
ranked_post = sorted(
    [(audio, sr, feats, score, idx)
     for (audio, sr, feats, idx), score in zip(pre_clips, pre_scores)],
    key=lambda x: -x[3]
)
# --- Display scores comparison ---
plt.figure(figsize=(6,4))
plt.bar(range(len(pre_scores)), pre_scores, color='skyblue')
plt.xlabel("Clip index")
plt.ylabel("Preference score (BT)")
plt.title("Preference Scores from Simulated Human Feedback (Bradley–Terry Model)")
plt.show()
```

该可视化突出了人类反馈的有效性——即使是模拟的反馈——在使生成模型与人类价值观对齐方面的作用。这个例子表明，RLHF 和人类反馈方法不局限于文本。通过收集偏好并训练简单的模型，我们可以在音频和音乐领域对齐生成输出。这种方法为跨模态的 AI 对齐开辟了新的可能性，确保生成的声音和音乐反映人类价值观和期望属性。正如文本对齐确保连贯的对话一样，音频对齐促进了情感和感知上的连贯性，使 RLHF 成为将多模态智能与人类意图对齐的统一框架。

## 总结

在本章中，我们探讨了 RLHF 和相关方法如何跨多种模态将生成模型与人类偏好对齐，扩展到文本之外的图像、运动轨迹以及音频或音乐。通过收集人类（或模拟）偏好分数并训练对齐模型，我们展示了生成输出如何能更好地反映人类价值观和期望品质。通过可视化和完整的实操示例，您深入理解了反馈驱动训练如何增强模型对齐。这些例子凸显了人类反馈在塑造 AI 行为方面的力量，并突显了这些技术在构建多模态、人类对齐系统方面的广阔潜力。

## 参考文献

*   AdvBench: Benchmarking Robustness to Adversarial Prompts in LLMs: <https://arxiv.org/abs/2312.06607>
*   JailbreakEval: A Benchmark for Jailbreak Robustness of Aligned Language Models: <https://arxiv.org/abs/2311.14852>
*   TruthfulQA: Measuring How Models Mimic Human Falsehoods: <https://arxiv.org/abs/2109.07958>
*   Ranking Game for Imitation Learning: <https://www.microsoft.com/en-us/research/publication/a-ranking-game-for-imitation-learning/>
*   RAPL (Representation-Aligned Preference Learning, Tian et al., 2024): <https://arxiv.org/pdf/2412.04835>
*   PromptCraft-Robotics & ChatGPT for Robotics (Vemprala et al., 2023): <https://www.microsoft.com/en-us/research/articles/chatgpt-for-robotics/>
*   LLM-Personalize (COLING, 2025): <https://arxiv.org/abs/2404.14285>
*   TrafficRLHF (NVIDIA, 2024): <https://arxiv.org/abs/2309.00709>
*   nuScenes: <https://www.nuscenes.org/>
*   Multimodal Driving RLHF (Rutgers University, 2025): <https://dl.acm.org/doi/10.1145/3675094.3677588>

