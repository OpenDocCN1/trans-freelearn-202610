where φ(·) is a perceptual feature extractor (typically VGG).

**Decoding Inverted Face Representations:** The function decode() uses the pSp decoder (built upon the StyleGAN generator) to reconstruct a face image from its latent code, intermediate features, and noise maps, and converts the output into a displayable RGB image.

```
def decode(model, w, f, noise):
    with torch.no_grad():
        img, _ = model.decoder([w], input_is_latent=True,
            first_layer_feature=f, noise=noise)
    img = (img.clamp(-1,1) + 1) * 127.5
    img = img.permute(0,2,3,1)[0].cpu().numpy().astype(np.uint8)
    return Image.fromarray(img)
```

**Face Morphing via Latent Interpolation:** First, both faces are aligned and inverted into the W⁺ latent space, producing latent representations w₁ and w₂. The morphing sequence is then created by linearly interpolating between these latent codes:

w(α) = (1 − α)w₁ + αw₂,    α ∈ [0,1]

Each interpolated latent vector is decoded by the pSp/StyleGAN generator to produce an intermediate face image, and the resulting frames are assembled into an animated GIF. The relevant code is:

```
w1, f1, n1, _, _ = invert(img1, psp, lm, inv_steps)
w2, f2, n2, _, _ = invert(img2, psp, lm, inv_steps)
frames = []
for a in np.linspace(0, 1, steps):
    w = (1-a)*w1 + a*w2
    frames.append(np.array(decode(psp, w, f1, n1)))
imageio.mimsave("morph.gif", frames, fps=15)
```

Here, invert() maps each face into the StyleGAN (W⁺) latent space, w = (1 − a)w1 + a * w2 performs latent interpolation, decode() reconstructs the corresponding face image, and mimsave() combines all generated frames into a smooth morphing animation. This produces temporally smooth face transformations, forming a continuous trajectory in latent space rather than pixel space. The resulting sequence illustrates the semantic continuity of the StyleGAN latent manifold, where facial identity changes gradually (from the left face to the right face) while preserving photorealism.

StyleGAN Morph Progression

![](bbox=[0.13, 0.8, 0.9, 0.9])

*Figure 13.14 – Face morphing with StyleGANEx*