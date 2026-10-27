```
def lomography_effect(img_path):
    img = cv2.imread(img_path)
    img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)  # Convert to RGB

    # Vignette Effect
    img = apply_vignette(img)

    # Increase Saturation
    pil_img = Image.fromarray(img)
    enhancer = ImageEnhance.Color(pil_img)
    img = np.array(enhancer.enhance(1.3))  # 30% saturation incr

    # Increase Contrast
    enhancer = ImageEnhance.Contrast(Image.fromarray(img))
    img = np.array(enhancer.enhance(1.5))  # 50% contrast incr

    return Image.fromarray(img)

# Apply effect and display
lomo_img = lomography_effect("images/pyramid.png")
lomo_img
```

If you run the preceding code snippet, you should obtain something similar to the following figure:

![](bbox=[0.22, 0.55, 0.83, 0.79])

Figure 3.13 – Applying a Lomography effect with PIL

# Practical applications

Vignetting and Lomography-style effects are widely used in mobile photography apps, retro filters, and social media platforms such as Instagram to enhance visual storytelling. These effects draw attention to the subject and evoke a nostalgic or dramatic mood.