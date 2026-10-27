Classical Högbom CLEAN also has limitations: it assumes sparse point sources and struggles with extended structures, PSF mismatch, or poor stopping rules, often producing artifacts or biases. To address this, many variants exist, such as Clark and Cotton–Schwab CLEAN for efficiency and wide-field imaging, multi-scale and wavelet CLEAN for extended sources, and more recent formulations that integrate sparse recovery, regularization, or Bayesian priors for improved robustness.

Inverse and Wiener filters operate globally in the frequency domain and assume linear models with smooth priors; they are suited for extended smooth signals but can struggle with sidelobe confusion and strongly non-Gaussian noise. CLEAN is greedy/spatial, explicitly modeling discrete point sources and iteratively subtracting PSF-shaped patterns: it excels when the scene is sparse, and the PSF has strong sidelobes. From a signal-processing view, CLEAN ≈ matching pursuit with the dictionary = {shifted PSF atoms}; the restoring beam is analogous to post-processing the sparse code with a smoother basis function. CLEAN is an illuminating example of how algorithmic assumptions (sparsity, point sources) shape the form and success of restoration algorithms.

# Image restoration with sparse representations

Sparse representations build on the observation that natural-image patches lie near a low-dimensional manifold and can be approximated by a sparse linear combination of a small number of prototype atoms. Dictionary learning + sparse coding, therefore, offer a powerful and interpretable approach to denoising and other restoration tasks.

We extract overlapping patches $x_i \in \mathbb{R}^{p^2}$ from an image and assume each patch admits a sparse linear model in a dictionary $D \in \mathbb{R}^{p^2 \times K}$:

```
$x_i \approx D \alpha_i \text{ with } \| \alpha_i \|_0 \le s,$
```

where $\alpha_i$ is a sparse code and $s$ (sparsity) is the number of nonzeros allowed. For a fixed dictionary $D$, the sparse coding problem for a patch $x$ is

```
$\min_{\alpha} \| x - D \alpha \|_2^2 \quad \text{s.t.} \quad \| \alpha \|_0 \le s,$
```

which is NP-hard in general. Orthogonal Matching Pursuit (OMP) is a greedy algorithm that approximates this by iteratively selecting the dictionary atom most correlated with the current residual and then performing orthogonal projection to update coefficients. OMP yields a fast, widely used approximate solution and is what transform_algorithm='omp' implements.

Dictionary learning jointly estimates $D$ and sparse codes $A = [\alpha_1, ..., \alpha_N]$ by solving:

```
$\min_{D,A} \| X - D A \|_F^2 \text{ s.t. } \forall i \| \alpha_i \|_0 \le s,$
```

typically with alternating minimization:

+   - Fix $D$, compute sparse codes $A$ (e.g., OMP).
+   - Fix $A$, update $D$ (e.g., least-squares, column normalization).

sklearn.decomposition.DictionaryLearning implements this alternating strategy (a MOD-like/online implementation).