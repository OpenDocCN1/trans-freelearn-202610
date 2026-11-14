
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

Here, we generate audio from prompts and extract interpretable features—spectral centroid (brightness) and zero-crossing rate (energy)—as proxies for musical qualities like “calm” or “energetic.”

### Modeling Preferences: Bradley-Terry Approach

To simulate RLHF, we collect pairwise preferences between audio clips and use the **Bradley-Terry model** to learn feature weights that best explain these preferences:

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

This approach allows us to rank future generations according to learned human-like criteria, aligning outputs with desired properties.

### Candidate Generation and Ranking

We generate multiple candidate audio clips for a prompt, extract their features, and simulate pairwise preferences. The learned preference model is then used to rank the candidates:

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

This process demonstrates how RLHF can guide generative models in non-language domains, using human feedback to shape outputs.

### Visualizing Preference Scores

To illustrate the impact of RLHF, we visualize the preference scores assigned to each candidate, showing how the learned model distinguishes between different audio outputs:

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

This visualization highlights the effectiveness of human feedback—even when simulated—in aligning generative models with human values. This example demonstrates that RLHF and human feedback methods are not limited to text. By collecting preferences and training simple models, we can align generative outputs in audio and music domains. This approach opens new possibilities for AI alignment across modalities, ensuring that generated sound and music reflect human values and desired properties. Just as text alignment ensures coherent dialogue, audio alignment fosters emotional and perceptual coherence, making RLHF a unifying framework for aligning multimodal intelligence with human intent.

## Summary

In this chapter, we explored how RLHF and related methods can align generative models with human preferences across multiple modalities, extending beyond text to images, motion trajectories, and audio or music. By collecting human (or simulated) preference scores and training alignment models, we demonstrated how generative outputs can better reflect human values and desired qualities. Through visualizations and complete hands-on examples, you gained an in-depth understanding of how feedback-driven training enhances model alignment. These examples underscore the power of human feedback in shaping AI behavior and highlight the broad potential of these techniques for building multimodal, human-aligned systems.

## References

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
