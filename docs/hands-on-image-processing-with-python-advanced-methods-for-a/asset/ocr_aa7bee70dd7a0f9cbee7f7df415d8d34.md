# Local Latent Perturbation and Manifold Structure

Local exploration of the latent space is performed by perturbing a center latent vector:

$$z' = z + \Delta z$$

where only selected dimensions are modified:

```
```python
delta[0] = (i-grid//2)*0.5
delta[1] = (j-grid//2)*0.5
z = center + delta
```
```

Early latent dimensions often control **global attributes** (pose, gender, lighting). Small perturbations reveal that StyleGAN3 encodes a **locally smooth manifold**, supporting controllable synthesis.

# Style Mixing in StyleGAN3

StyleGAN introduces a **style-based generator architecture**, where different layers control different levels of abstraction. The synthesis process is:

$$w=f(z),\quad x=G_s(w)$$

Style mixing replaces subsets of latent styles:

$$w_l^{(mix)}=\begin{cases}w_l^{(A)}, & l<k\\ w_l^{(B)}, & l\ge k\end{cases}$$

The equation defines StyleGAN style mixing where early layers ($l<k$) use style from latent $A$ and later layers ($l\ge k$) use style from latent $B$, producing a hybrid image combining coarse structure from $A$ and fine details from $B$.

This is implemented with the following code, which performs StyleGAN style mixing by generating latent vectors from row and column seeds, combining early-layer styles from one vector with later-layer styles from another, and synthesizing a grid of synthetic face images to visualize controlled semantic feature transfer.

```
```python
W_row, W_col = [], []
for s in [10,20,30]:
    z = torch.from_numpy(np.random.RandomState(s).randn(1,G.z_dim)).to(device)
    W_row.append(G.mapping(z, None))
for s in [100,200,300]:
    z = torch.from_numpy(np.random.RandomState(s).randn(1,G.z_dim)).to(device)
    W_col.append(G.mapping(z, None))

for r in range(3):
    for c in range(3):
        w = W_row[r].clone()
        w[:,8:] = W_col[c][:,8:]
        img = G.synthesis(w)
```
```