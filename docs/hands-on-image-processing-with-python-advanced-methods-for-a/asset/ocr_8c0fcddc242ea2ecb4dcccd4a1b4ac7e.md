# QUALITY CONTROL REPORT

# Overall Quality: poor

Multiple defects identified including cracks, deformations, and surface contamination affecting capsule integrity and appearance.

![](bbox=[0.21, 0.16, 0.52, 0.27])

#1 crack
Severity: high
Confidence: 95.0%
Location: first capsule
Visible crack running down the length of the capsule.

#2 deformation
Severity: medium
Confidence: 90.0%
Location: second capsule
Surface deformation visible on the red portion of the capsule.

#3 surface contamination
Severity: medium
Confidence: 85.0%
Location: third capsule
Brownish discoloration visible on the lower part of the capsule.

Defects Detected: 3

Figure 13.47 – Detecting manufacturing defects with GPT-image

This demonstrates how multimodal foundation models can assist industrial quality assurance.

# Image colorization

GPT Image can convert grayscale photographs into realistic color images.

```
prompt = """Colorize this grayscale photograph. Preserve: geometry, faces, objects, composition.Add realistic natural colors. Photorealistic. High quality. Historical accuracy if applicable."""

input_image = "images/balloons.png"
with open(input_image, "rb") as img:
    result = client.images.edit(model="gpt-image-1", image=img, prompt=prompt)
```

The prompt instructs the model to preserve geometry and content while generating plausible colors.

![](bbox=[0.26, 0.64, 0.77, 0.84])

Figure 13.48 – Colorising a grayscale image with GPT-image