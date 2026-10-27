```
```python
analysis = response.output_text
print("\nGPT Vision Analysis:")
print(analysis)
```
```

> **Summary**:
> This is a heavily blurred vintage image with low noise and no obvious scratches. Restoration would mainly focus on improving sharpness, contrast, and reconstructing key facial features to bring back clarity and character to the photograph.

# Stage 2: Restoration:

```
```python
result = client.images.edit(model="gpt-image-1", image=[("old_photo.jpg", img_bytes)], prompt="""Restore this photograph. Remove blur. Remove noise.

Improve facial detail. Preserve identity.""")
```
```

The model enhances facial details while preserving the original identity and overall appearance.

![](bbox=[0.3, 0.433, 0.748, 0.616])

*Figure 13.43 – Face restoration with GPT vision*

Applications include historical photograph restoration, family photo enhancement, archive digitization, and face reconstruction.

# Multi-image composition

GPT Image can jointly reason over multiple input images, as shown in the next code:

```
```python
result = client.images.edit(model="gpt-image-1",
    image=[open("images/beach.png", "rb"), open("images/whale.jpg", "rb")],
    # image=[open("images/giraffe.jpg", "rb"), image=[open("images/bird.jpg", "rb")]
    prompt="Combine these into a realistic product scene")
```
```

The model merges visual concepts from multiple sources (whale and beach, giraffe and bird) into a coherent composition, as shown in the next figure.