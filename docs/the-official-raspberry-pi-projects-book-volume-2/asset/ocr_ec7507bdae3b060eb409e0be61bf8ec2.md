own pixel art in something like Swanky Paint. For testing this tutorial, we grabbed one of the 100 characters from a sprite sheet created by Johan Vinet (twitter.com/johanvinet), which can be found here: bit.ly/1jxaJ3m.

# >STEP-03
Prepare the art

The Sense HAT can only display 64 pixels, and our code assumes the image you have is going to be exactly that. On a normal computer, use an image editor such as the free software GIMP to prepare the image. Remove any borders (you may need to zoom right in to make sure) by cropping the image and save it to a file name you’ll remember with the extension .png or .gif. This is especially important if the 8×8 art you have isn’t actually saved as an 8×8 image; for instance, if each ‘pixel’ in the art is six pixels wide, like in our code.

# >STEP-04
Study the pixel art

As mentioned above, your pixel art may not be actually saved by the pixel. In the code we’ve created, it takes this into account. The easiest way to check the width of the pixels is to look at the resolution of the image. In our case, it’s 48×48, which means that each ‘pixel’ is six real pixels wide (and high, because it’s a square). You can check this in GIMP by using a square pixel brush and increasing the size until it’s the same as the pixels in your image. Alternatively, once you’ve cropped the image, you can scale the image to be 8×8; this may not have the desired results, though, as it tries to squash everything together.

# >STEP-05
Load it all on the Raspberry Pi

Put the image on the Raspberry Pi and open it in the image viewer to make sure it transferred fine. We did this by plugging the SD card directly into the computer we prepared the image on and copied it to a directory called ‘SenseHAT’ in the home folder, but you can always put it on a USB stick and copy it over from within the Pi or upload it to your cloud storage and download it to the Pi.

# >STEP-06
Write the code!

Follow along to the code listing and tweak it to suit your own pixel art, whether this means you need to change the location of the file in the code (our code assumes you’ve put it in a folder called ‘SenseHAT’ in Raspbian’s home directory) or if you need to change the width of each pixel to 1 or higher. Press F5 to run the code, and it will then display the image on the Sense HAT for three seconds before turning itself off.

# Pixelart.py

```
from sense_hat import SenseHat
import time
from PIL import Image
import os

# Open image file
image_file = os.path.join(
os.sep, "/home", "pi", "SenseHAT", "pixelart.png")
img = Image.open(image_file)

# Generate rgb values for image pixels
rgb_img = img.convert('RGB')
image_pixels = list(rgb_img.getdata())

# Get the 64 pixels you need
pixel_width = 6
image_width = pixel_width*8
sense_pixels = []
start_pixel = 0
while start_pixel < (image_width*64):
    sense_pixels.extend(image_pixels[start_pixel:(
start_pixel+image_width):pixel_width])
    start_pixel += (image_width*pixel_width)

# Display the image
sense = SenseHat()
sense.set_rotation(r=180)
sense.set_pixels(sense_pixels)
time.sleep(3)

sense.clear()
```

![](bbox=[0.43, 0.62, 0.93, 0.9])

Above Rob is very vain, and (carefully) cut out an image of his alter-ego M. Bison to display on the Sense HAT, taken from Johan's sprite list