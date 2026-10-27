
If you run the preceding code snippet, you should obtain a figure like the following:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_149_0.png)

Figure 3.10 – Exchanging color palettes of images with PIL

Note:
When using putpalette(), ensure that the palette length does not exceed 768 values (256 colors × 3 channels). Assigning a longer palette may raise an error. This is especially important when working with dynamically generated or unbounded palettes.

## Adjusting the temperature of an image

The color temperature of an image refers to its overall color tone, measured in Kelvins (K). It represents the warmth (reddish/yellowish tones) or coolness (bluish tones) of an image:

- Lower temperatures (1,000K – 4,000K): Warmer, reddish tones (e.g., candlelight, sunrise)
- Neutral temperature (5,500K): Natural daylight
- Higher temperatures (6,000K – 10,000K): Cooler, bluish tones (e.g., overcast sky, shade)

For example, a candle flame (1900K) is warm (orange/yellow), while a blue sky (8000K) is cool.

Color temperature adjustment is done by scaling the RGB channels using a transformation based on the desired temperature shift.

## Adjusting the temperature using RGB scaling

A simple way to change an image's temperature is to scale the red, green, and blue channels based on the target temperature.

For an input image, I(x, y) = (R, G, B), we adjust it using:

```
R' = R × α_R, G' = G × α_G, B' = B × α_B
```

where α_R, α_G, and α_B are scaling factors based on the Kelvin temperature.

The following is an example:

- **Warmer shift (increase red, decrease blue)**: α_R > 1, α_G ≈ 1, α_B < 1
- **Cooler shift (increase blue, decrease red)**: α_R < 1, α_G ≈ 1, α_B > 1

## Adjusting the temperature using a color transformation matrix

A more precise way of adjusting the temperature is to use a color transformation matrix:

```
M = [[α_R, 0, 0, 0], [0, α_G, 0, 0], [0, 0, α_B, 0]]
```

Applying the matrix transformation to the image we obtain: I' = M · I.

This transforms each (R, G, B) pixel based on the Kelvin temperature correction factors.

For 3,000K (warmer shift), red remains unchanged (scaled by 1.0), green is reduced to 70.5% of its original value, and blue is reduced to 42%, making the image appear warmer.

For 8,000K (cooler shift), red is reduced (0.89) to cool the image, green is slightly adjusted (0.95), and blue is enhanced (1.1) to create a cooler effect.

The RGB equivalents of a temperature can be found here: http://www.vendian.org/mncharity/dir3/blackbody/.

Now, let's change an image's color temperature using Kelvin-based RGB transformations, using the next code snippet. Here are the steps:

- The code uses a predefined kelvin_table with RGB values.
- It constructs a color transformation matrix.
- It applies the matrix using PIL's Image.convert() method.

The next figure visualizes the RGB tone changes across different color temperatures, provided by kelvin_table in the following code snippet:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_150_0.png)

Figure 3.11 – RGB tone changes across color temperatures

The convert(mode, matrix) method in PIL allows linear color transformations using a transformation matrix M. It applies a linear transformation to each pixel's RGB values. Given an input pixel (R, G, B), it transforms it into a new output pixel (R', G', B'), as follows:

```
[R']   [α_R 0 0 0] [R]
[G'] = [0 α_G 0 0]·[G]
[B']   [0 0 α_B 0] [B]
                     [1]
```

This transformation applies independently to every pixel in the image. It can be used to simulate different lighting conditions (e.g., candlelight, daylight, or cool fluorescent lighting) and is thus useful for photo color correction and artistic effects.

Now, refer to the following code snippet:

```
from PIL import Image

kelvin_table = {
    1000: (255,56,0),
    1500: (255,109,0),
    2000: (255,137,18),
    2500: (255,161,72),
    3000: (255,180,107),
    3500: (255,196,137),
    4000: (255,209,163),
    4500: (255,219,186),
    5000: (255,228,206),
    5500: (255,236,224),
    6000: (255,243,239),
    6500: (255,249,253),
    7000: (245,243,255),
    7500: (235,238,255),
    8000: (227,233,255),
    8500: (220,229,255),
    9000: (214,225,255),
    9500: (208,222,255),
    10000: (204,219,255)}

def convert_temp(image, temp):
    r, g, b = kelvin_table[temp]
    matrix = ( r / 255.0, 0.0, 0.0, 0.0,
               0.0, g / 255.0, 0.0, 0.0,
               0.0, 0.0, b / 255.0, 0.0 )
    return image.convert('RGB', matrix)

im = Image.open('images/boy.jpg').convert('RGB')
#print(im.mode)
plt.figure(figsize=(12,4))
plt.subplot(141), plt.imshow(im), plt.title('original', size=15)
for i, k in enumerate([1000, 5000, 10000]):
    plt.subplot(1,4,i+2), plt.imshow(convert_temp(im, k))
    plt.title(f'{k}K', size=15), plt.axis('off')
```

If you run the preceding code snippet, you should obtain something the following figure:

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_152_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_152_1.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_152_2.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_152_3.png)

Figure 3.12 – Changing temperature of an image with PIL

## Implementing vignetting/Lomography effect

Vignetting is a visual effect where the edges of an image are darkened, creating a natural spotlight on the center, whereas the Lomography effect is a retro, high-contrast, oversaturated look often associated with vintage film cameras.

Next python demonstration creates a Lomography-style effect by combining vignetting, increased saturation, and enhanced contrast.

The apply_vignette() function generates a radial mask using the distance of each pixel from the image center, vignette = clip(1 − αd^p ,0,1) where alpha controls vignette strength and p controls the falloff rate.

The mask is multiplied with each RGB channel to darken the image edges while preserving the center. The lomography_effect() function loads the image with cv2.imread(), converts it to RGB using cv2.cvtColor(), applies the vignette, then boosts color intensity with ImageEnhance.Color().enhance(1.3) (30% saturation increase) and contrast with ImageEnhance.Contrast().enhance(1.5) (50% contrast increase). The resulting image exhibits the characteristic Lomography look with darker borders, richer colors, and stronger contrast that draws attention to the central subject.

```
from PIL import ImageEnhance

def apply_vignette(img, alpha=0.7, p=2.5):
    rows, cols = img.shape[:2]
    x, y = np.meshgrid(np.linspace(-1, 1, cols), \
                       np.linspace(-1, 1, rows))
    d = np.sqrt(x**2 + y**2)
    vignette = 1 - alpha * (d ** p)
    vignette = np.clip(vignette, 0, 1).astype(np.float32)

    for i in range(3):  # Apply to R, G, B channels
        img[:, :, i] = img[:, :, i] * vignette
    return img
```

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

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_153_0.png)

Figure 3.13 – Applying a Lomography effect with PIL

## Practical applications

Vignetting and Lomography-style effects are widely used in mobile photography apps, retro filters, and social media platforms such as Instagram to enhance visual storytelling. These effects draw attention to the subject and evoke a nostalgic or dramatic mood.
