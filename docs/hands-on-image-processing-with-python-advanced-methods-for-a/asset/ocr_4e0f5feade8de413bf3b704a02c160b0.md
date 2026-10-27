# Block views on images

Block views using skimage.util.view_as_blocks() split an image into non-overlapping patches to enable local operations such as pooling. Given a grayscale image partitioned into m × n blocks (after optional padding to ensure divisibility), each block is reduced to a single value to form a downsampled image: mean pooling computes the average intensity, max pooling selects the maximum intensity, and median pooling selects the median intensity within each block.

The next code block defines couple of helper functions for visualization: plot_image() displays a single image with a title and axis turned off, while plot_images() arranges multiple images in a 1×n grid using matplotlib, applies grayscale colormap, sets individual subplot titles, and adds a global suptitle with adjustable figure and font sizes.

```
def plot_image(im, title='', fontsize=20):
    plt.imshow(im), plt.axis('off'),
    plt.title(title, size=fontsize)

def plot_images(images, titles, suptitle='', fontsize=20, \
                supfontsize=22, figsize=(15,7)):
    n = len(images)
    plt.figure(figsize=figsize)
    plt.gray()
    for i in range(n):
        plt.subplot(1,n,i+1), plot_image(images[i], titles[i], fontsize)
    plt.suptitle(suptitle, size=supfontsize)
    plt.tight_layout()
    plt.show()
```

The next code snippet uses different pooling methods for implementing a block view for an input image:

```
from skimage.util import view_as_blocks
im = color.rgb2gray(imread('images/sculpture.png')[..., :3])
block_shape = (8, 8) # Ensure image dimensions are divisible by block size
pad_height = (block_shape[0] - im.shape[0] % block_shape[0]) % block_shape[0]
pad_width = (block_shape[1] - im.shape[1] % block_shape[1]) % block_shape[1]
if pad_height != 0 or pad_width != 0:
    # optional: pad the image to make it divisible by block size
    im = np.pad(im, ((0, pad_height), (0, pad_width)), mode='reflect')
view = view_as_blocks(im, block_shape)
flatten_view = view.reshape(view.shape[0], view.shape[1], -1)
mean_view = np.mean(flatten_view, axis=2)
max_view = np.max(flatten_view, axis=2)
median_view = np.median(flatten_view, axis=2)
plot_images([im, mean_view, max_view, median_view], ['original', \
    'local mean pooling', 'local max pooling', 'local median pooling'])
```