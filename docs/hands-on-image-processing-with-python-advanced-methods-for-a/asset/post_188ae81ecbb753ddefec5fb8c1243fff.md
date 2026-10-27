
## Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here: https://packt.link/gbp/9781837636235.

## Conventions used

There are a number of text conventions used throughout this book.

CodeInText: Indicates code words in text, database table names, folder names, filenames, file extensions, pathnames, dummy URLs, user input, and Twitter handles. For example: “The following sections demonstrate the implementation of a few nonlinear filters with PIL, scikit-image, and scipy.ndimage library functions.”

A block of code is set as follows:

```
def histogram_sliding(image, shift_value=30):
    slid = np.clip(image + shift_value, 0, 255)
    return slid.astype(np.uint8)

image = cv2.imread('images/mandrill.jpg')
image = cv2.cvtColor(image, cv2.COLOR_BGR2LAB)
stretched_image = image.copy()
shrunk_image = image.copy()
```

Any command-line input or output is written as follows:

```
! python -m streamlit run app.py
```

Bold: Indicates a new term, an important word, or words that you see on the screen. For instance, words in menus or dialog boxes appear in the text like this. For example: “This allows the filter to reduce noise in homogeneous regions while preserving sharp edges, making it particularly effective for natural images where structure and detail must be retained.”

Warnings or important notes appear like this.

Tips and tricks appear like this.

## Get in touch

Feedback from our readers is always welcome.

**General feedback:** If you have questions about any aspect of this book or have any general feedback, please email us at customercare@packt.com and mention the book's title in the subject of your message.

**Errata:** Although we have taken every care to ensure the accuracy of our content, mistakes do happen. If you have found a mistake in this book, we would be grateful if you reported this to us. Please visit http://www.packt.com/submit-errata, click **Submit Errata**, and fill in the form.

**Piracy:** If you come across any illegal copies of our works in any form on the internet, we would be grateful if you would provide us with the location address or website name. Please contact us at copyright@packt.com with a link to the material.

**If you are interested in becoming an author:** If there is a topic that you have expertise in and you are interested in either writing or contributing to a book, please visit http://authors.packt.com/.

## Share your thoughts

Once you’ve read *Hands-On Image Processing and Computer Vision with Python*, we’d love to hear your thoughts! Please click here to go straight to the Amazon review page for this book and share your feedback.

Your review is important to us and the tech community and will help us make sure we’re delivering excellent quality content.

## How to Unlock

Scan the QR code (or go to packtpub.com/unlock). Search for this book by name, confirm the edition, and then follow the steps on the page.

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_32_0.png)

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_32_1.png)

*Note: Keep your invoice handy. Purchases made directly from Packt don’t require one*

## Join our Discord and Reddit Space

You’re not the only one navigating fragmented tools, constant updates, and unclear best practices. Join a growing community of professionals exchanging insights that don’t make it into documentation.

| Join our Discord at https://packt.link/I1tSU or scan the QR code below: [QR code] | Follow us on Reddit at https://packt.link/0rExL or scan the QR code below: [QR code] |

## Part 1

## Foundations of Digital
Image Processing

Part 1 builds the essential mathematical, algorithmic, and computational foundations required for all subsequent topics in the book. It begins with the fundamentals of digital images, including representation, sampling, quantization, and image formation, and progresses toward practical image manipulation using Python. Readers are introduced to core programming workflows using NumPy, PIL, OpenCV, and scikit-image for reading, transforming, and visualizing images. The part then develops a strong theoretical and applied understanding of spatial and frequency-domain analysis. Topics include Fourier transforms, discrete convolution, correlation, filtering theory, and the relationship between spatial and spectral representations of images. Emphasis is placed on understanding how image structure can be analyzed and modified using mathematical operators.

Finally, classical image enhancement and filtering techniques are introduced, including smoothing, sharpening, and basic restoration concepts. By the end of this part, readers will have a solid foundation in both the mathematical principles and practical tools required to work effectively with digital images and prepare for more advanced computer vision and deep learning concepts.

This part contains the following chapters:

- *Chapter 1, Getting Started with Digital Image Processing*
- *Chapter 2, Image Manipulation*
- *Chapter 3, More Image Manipulation*
- *Chapter 4, Sampling and Fourier Transform*
- *Chapter 5, Convolution and Spatial/Frequency Domain Filtering*
- *Chapter 6, Frequency Domain Filtering*

## 1

## Getting Started with Digital Image Processing

Digital image processing is defined as the application of computational algorithms for the acquisition, representation, transformation, analysis, and interpretation of images in discrete digital form. A digital image is modeled as a finite-dimensional numerical array, and processing operations are formulated as mappings on such arrays for tasks such as enhancement, restoration, feature extraction, and interpretation. Thus, it forms the computational basis for extracting meaningful information from visual data.

Digital image processing has wide applications in television systems, photography, robotics, remote sensing, medical imaging, and industrial inspection. Modern large-scale platforms, including social media systems such as Facebook and Instagram, also employ image processing techniques for compression, enhancement, filtering, and automated content analysis.

This book employs Python libraries for both classical and deep learning-based image processing. Classical methods cover image acquisition, transformation, preprocessing, enhancement, restoration, representation, segmentation, classification, detection, and recognition. Deep learning frameworks are additionally introduced to enable data-driven approaches for advanced image processing tasks, reflecting recent developments in the field.

This chapter introduces fundamental concepts including pixels, color spaces, and image representations, along with the standard image processing pipeline. It further covers image input/output, visualization, array-based data structures, image types, file formats, and basic manipulation techniques, focusing on computational representations used in modern systems.

By the end of this chapter, the reader will understand core principles of digital image processing, be able to use Python-based libraries for image I/O and manipulation, and interpret common image representations and formats.

The chapter is organized as follows:

- Image processing and applications
- Image input/output and visualization in Python
- Image types, file formats, and data representations
- Color spaces and transformations
- Basic image manipulation techniques

![](img/ebc2ad05fd41bdb1531c3dba43fbf8d5_36_0.png)

### Technical requirements

This chapter is designed to be followed interactively using **Jupyter Notebook**. The notebooks and code corresponding to this chapter are available in the GitHub repository: https://github.com/PacktPublishing/Hands-on-Image-Processing-and-Computer-Vision-with-Python-Second-Edition. Readers can clone the repository, navigate to the Chapter01 folder, open the notebook, and execute the cells to reproduce the results.

The introductory sections and notebooks discuss the required software setup, including Python installation, library dependencies, and environment configuration needed for later chapters. All required input images, auxiliary files, and pretrained models are provided in the repository, typically within chapter-specific images and models subfolders.

The book emphasizes developing a deep conceptual understanding and intuition behind image processing and computer vision algorithms rather than focusing solely on highly optimized implementations. Multiple approaches to solving the same problem are explored to illustrate the evolution, strengths, and limitations of different methods and algorithms.

### Reproducibility and version compatibility

This book provides tested, hands-on Python implementations for image processing and computer vision using libraries such as NumPy, OpenCV, and scikit-image. All examples were verified with specific library and Python versions listed in the book and accompanying GitHub repository to ensure reproducibility.

Since the Python ecosystem evolves rapidly, newer library versions may introduce API changes, deprecations, or breaking modifications that can affect code execution. To minimize compatibility issues, readers are encouraged to:

- Use the recommended Python and library versions specified in the book.
- Create isolated environments using tools such as `venv`, `conda`, or `requirements.txt` for reproducible setups.
- Use utilities such as `pip freeze` to replicate working environments.
