
2. **Initialization:** Mask (mask) is initialized to zeros (background). The mask image is an image through which we can specify which regions in the image are:

- Surely background: with value 0 (or the flag `cv2.GC_BGD`)
- Surely foreground: with value 1 (or the flag `cv2.GC_FGD`)
- Probable background/foreground: using the following flags `cv2.GC_PR_BGD` (with value 2) and `cv2.GC_PR_FGD` (with value 3).

3. **Define Models:** `bg_model` and `fg_model` – internal GMM parameters for background and foreground, respectively.

4. **Provide Initial Hint:** Provide the coordinates of a rectangle (`rect`) around the flowers to provide a (rough) hint to the algorithm that the rectangle defines **probable foreground region**.

5. **Run GrabCut:** Call opencv-Python's `grabCut()` function to segment the foreground flowers (objects) from the input image, with the following parameters

- The input image, the blank mask and the foreground rectangle coordinates.
- The mode parameter set to `cv2.GC_INIT_WITH_RECT`.
- Number of iterations to run the algorithm (5 here), it performs 5 iterations of: *GMM estimation* *graph-cut optimization* and it modifies the mask image provided.

6. **Extract Final Mask:** Modify the mask so that all 0/2 valued pixels are set 0 (background) and all 1/3 valued pixels are set to 1 (foreground).

7. **Apply Mask:** Multiply with the input image with the final mask to extract foreground object and obtain the segmented image, as shown in the next code snippet.

```
import cv2

img = cv2.imread('images/dalhia.jpg')
img_orig = img.copy()
mask = np.zeros(img.shape[:2],np.uint8)
bg_model = np.zeros((1,65),np.float64)
fg_model = np.zeros((1,65),np.float64)
rect = (200,20,720,680) #(80,100,720,550)

cv2.grabCut(img, mask, rect, bg_model, fg_model, 5,
            cv2.GC_INIT_WITH_RECT)
mask2 = np.where((mask==2)|(mask==0), 0, 1).astype('uint8')
cv2.rectangle(img_orig, rect[:2], rect[-2:], (255,0,0), 2)
img = img*mask2[:,:, np.newaxis]
plt.imshow(np.hstack((cv2.cvtColor(img_orig, cv2.COLOR_BGR2RGB), \
            cv2.cvtColor(img, cv2.COLOR_BGR2RGB)))), plt.colorbar()
```

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_505_0.png)

## Figure 10.11 – Image segmentation with GrabCut (using mask)

The preceding figure shows the input image along with the bounding box rectangle drawn around the foreground object and the segmented object using the GrabCut algorithm, by running the above code snippet. Notice that the initial rectangle misses a small part of the flowers and includes a part of the background of the image.

> Note
> The output depends on the initial rectangle. Errors occur if the object is not fully inside the rectangle or background included. Refinement can be done via manual strokes (mask-based initialization).

From the output image, we can readily see the extracted foreground. As can be seen, a part of the background is also (wrongly) included inside the segmented output. This can be corrected by providing a mask with manual strokes defining a few (wrongly identified) foreground and background pixels (mask-based initialization). The next line of code loads a mask image (refer to the book's accompanying GitHub repo) where a few such strokes (scribbles) are provided (green strokes marking a few foreground and blue strokes marking a few background pixels):

```
newmask = cv2.imread('images/dahlia_mask.png')
```

Now the algorithm needs to be run with this mask. The next code block shows how to run the cv2.grabCut() function with this mask. The following figure shows the output of the code block, the correctly segmented foreground object with the algorithm, with just the strokes in the mask as hints:

```
mask = 2*np.ones(img.shape[:2],np.uint8)
# blue scribble pixels are surely backgrounds
mask[(newmask[...,0] >= 200)&(newmask[...,1] <= 20)&(newmask[...,2]<=20)] = 0 #
# green scribble pixels are surely foregrounds
mask[(newmask[...,0] <= 20)&(newmask[...,1] >= 200)&(newmask[...,2]<=20)] = 1
mask, bg_model, fg_model = \
    cv2.grabCut(img,mask,None,bg_model,fg_model,5,cv2.GC_INIT_WITH_MASK)
mask = np.where((mask==2)|(mask==0),0,1).astype('uint8')
img = img*mask[:,:,np.newaxis]
plt.imshow(cv2.cvtColor(img,cv2.COLOR_BGR2RGB)), plt.colorbar()
```

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_506_0.png)

As you can see, **GrabCut** solves segmentation as:

$$\text{Segmentation} = \arg\min E(f)$$

using **probabilistic modeling + graph optimization**.

## Conditional random fields

Conditional random fields (CRFs) model segmentation as a **probabilistic labeling problem with contextual dependencies**, where each pixel’s label depends not only on its own features but also on its neighbors.

### Probabilistic model

Intuitively, a CRF improves segmentation by refining each pixel’s label based on the agreement with its neighboring pixels, ensuring spatially coherent regions rather than independent per-pixel predictions.

Given image $I$, we model label assignment $\mathbf{X}=\{x_i\}$, where $x_i$ is label of pixel $i$ (e.g., foreground/background or semantic class) and $P(\mathbf{X}\mid I)=\frac{1}{Z}\exp(-E(\mathbf{X}))$, where $Z$: **partition function** (normalizes probabilities), and $E(\mathbf{X})$: **Gibbs energy**, measuring how good a labeling is, where low energy → high probability. CRF chooses the labeling that minimizes energy (maximum a posteriori, MAP).

**Energy function:**

$$E(\mathbf{X}) = \sum_i \psi_u(x_i) + \sum_{i<j} \psi_p(x_i,x_j),$$

where $\psi_u(x_i)$ – unary potential (data term): Measures how well the pixel $i$ fits label $x_i$. It is often derived from a classifier (e.g., CNN output or annotation), in the following implementation, it is derived directly from the annotation labels.

$\psi_p(x_i,x_j)$ – pairwise potential: Encourages spatial consistency between pixels.

Unary → “what this pixel wants to be” and Pairwise → “what its neighbors want it to be”.

2. **Convert annotation to labels:** Converts RGB → discrete labels $x_i \in \{1,2, ..., K\}$ (integer classes), where $K$ is the number of unique labels in the annotation. This defines the **label space** of the CRF.

3. **Handle unknown labels:** Some pixels may have **unknown labels** (HAS_UNK = 0 in colors). These contribute weakly to unary term. CRF relies more on **pairwise terms** for them.

4. **Initialize dense CRF model:** Fully connected CRF:

$E(\mathbf{X}) = \sum_i \psi_u(x_i) + \sum_{i<j} \psi_p(x_i, x_j)$

where every pixel interacts with **every other pixel**, the DenseCRF() class constructor instantiates a **CRF** object, as shown in the next code snippet.

5. **Unary potential (Data Term):**

$\psi_u(x_i) = -\log P(x_i)$

derived from annotation using the function unary_from_labels(), with the confidence controlled by gt_prob (0.7 in the code). Use the method setUnaryEnergy() to set **unary potentials** $\psi_u(x_i)$, to encode initial belief (data term) for each pixel label, where strong annotation → strong unary and weak/unknown → CRF can correct.

## **Pairwise potentials (smoothness and context)**

In practice, pairwise potentials are commonly implemented using *Gaussian* and bilateral *kernels*, which encourage *spatial consistency* while preserving important image boundaries and contextual relationships between pixels.

- **Gaussian Kernel (Spatial Smoothing):** Use the function create_pairwise_gaussian() to compute $\exp\left(-\frac{|p_i - p_j|^2}{2\sigma^2}\right)$, depends only on spatial distance. Use the method addPairwiseEnergy() to add it to the CRF.
- **Bilateral Kernel (Edge-Aware Smoothing):** Use the function create_pairwise_bilateral() to compute $\exp\left(-\frac{|p_i - p_j|^2}{2\sigma_a^2} - \frac{|I_i - I_j|^2}{2\sigma_b^2}\right)$. Use the method addPairwiseEnergy() again to add it to the CRF. If pixels are **close + similar color → strong interaction** and **different color → weak interaction**. It preserves edges while smoothing.

Use the method addPairwiseEnergy() to add a **pairwise potential term** $\psi_p(x_i, x_j)$ to the **CRF**. Here is how it works and the parameters it accepts are feats (feature vectors, e.g., spatial or bilateral features), compat (weight w, strength of smoothing: Computes similarity between pixels using Gaussian kernels and defines *how strongly pixels influence each other*). **Gaussian** → smooth nearby pixels (spatial smoothness), whereas **bilateral** → smooth only similar-color pixels (edge-preserving refinement), encourages **nearby pixels → same label** and removes noise.

**Mean-Field Inference:** It's an efficient approximation that converts hard optimization into iterative filtering and probabilistic updates. Use the method inference(n_iter) to run **mean-field inference** for n_iter (e.g., 5 iterations in the code) iterations and update:

$Q_i(x_i) \propto \exp\left(-\psi_u(x_i) - \sum_j \psi_p(x_i, x_j)\right)$
