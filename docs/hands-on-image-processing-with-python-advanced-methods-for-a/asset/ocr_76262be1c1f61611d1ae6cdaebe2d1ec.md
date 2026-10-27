Hence, correlation and convolution differ in whether the kernel is flipped before application. The next figure summarizes how this affects the output:

![](bbox=[0.12, 0.15, 0.83, 0.31])

Figure 5.9 – 2D correlation versus convolution – a numeric example

The scipy.signal module's correlate2d() function can be used for correlation. The following code can be used to verify the preceding manual computation, by visually demonstrating the difference between correlation and convolution outputs using annotated heatmaps using the seaborn library:

```
from scipy.signal import convolve2d, correlate2d
import seaborn as sns
k = np.reshape(range(1,5), (2,2))
I = np.reshape(range(1,17), (4,4))
conv_out = convolve2d(I, k, mode='valid')
corr_out = correlate2d(I, k, mode='valid')
sns.heatmap(conv_out, annot=True, fmt='d', linewidths=.5, cbar=False)
sns.heatmap(k, annot=True, fmt='valid', linewidths=.5, cbar=False)
```

You should obtain a plot like the following one if you run the preceding code.

![](bbox=[0.18, 0.7, 0.78, 0.9])

Figure 5.10 – 2D correlation versus convolution output