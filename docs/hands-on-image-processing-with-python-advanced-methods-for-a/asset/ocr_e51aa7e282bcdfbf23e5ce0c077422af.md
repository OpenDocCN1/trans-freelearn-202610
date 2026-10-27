# StyleGAN3 Synthetic Faces

![](bbox=[0.31, 0.08, 0.76, 0.38])

Figure 13.11 – Synthetic faces generated with StyleGAN3

Latent Space Interpolation: A key property of StyleGAN is that the latent space is semantically smooth. Given two latent codes (z₁, z₂), interpolation is defined as:

```
z(α) = (1 − α)z₁ + αz₂,      α ∈ [0,1]
```

This produces gradual semantic transitions such as identity morphing, pose transitions and expression blending.

The next code snippet generates two seed-based latent vectors and linearly interpolates between them across multiple steps, feeding each interpolated vector into StyleGAN to visualize a smooth transition between synthetic face images produced by the generator.

```
z1 = torch.from_numpy(np.random.RandomState(100).randn(1,G.z_dim)).to(device)
z2 = torch.from_numpy(np.random.RandomState(500).randn(1,G.z_dim)).to(device)
for alpha in np.linspace(0,1,9):
    z = (1-alpha)*z1 + alpha*z2
    img = G(z, torch.zeros([1,G.c_dim], device=device))
```

Unlike pixel-space interpolation, latent interpolation preserves realism because the generator maps the linear latent manifold to a nonlinear image manifold.

# Latent Space Interpolation

![](bbox=[0.14, 0.79, 0.86, 0.85])

Figure 13.12 – Latent space interpolation with StyleGAN3