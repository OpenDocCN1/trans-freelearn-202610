
## Chapter 12: Image Classification and Object Detection

- Technical requirements .......................................................... 566
- Visualizing high-dimensional image features with t-SNE ............................. 566
  - MNIST embeddings with t-SNE · 566
  - Visualizing deep features from VGG16 · 568
  - CNN brain viewer dashboard · 570
    - *Interactive exploration with Gradio* · 571
- Image classification .............................................................. 572
- Transfer learning for image classification ........................................ 573
  - Transfer learning with VGG16 (TensorFlow/Keras) · 573
    - *Approach 1: Using a pre-trained VGG16 network* · 574
    - *Approach 2: Transfer learning using VGG16* · 575
  - Transfer learning with VGG16 (PyTorch) · 579
- Image classification with attention networks ...................................... 580
  - CNN with channel and spatial attention · 580
  - Image classification with vision transformer (ViT) · 583
    - *Flower classification with ViT* · 584
  - Image classification with Swin Transformer · 585
- Few-shot learning ................................................................. 586
  - Zero-shot learning with CLIP · 586
  - Few-shot learning with DINOv2 · 587
  - Few-shot face recognition with deep face embeddings (Siamese network) · 589
    - *Improving recognition with Siamese fine-tuning* · 591
    - *Face recognition with FaceNet* · 592
- Bayesian neural networks for image classification .................................. 593
  - MC dropout: A practical Bayesian approximation · 595
- Image classification with semi-supervised learning ................................ 597
- Masked Autoencoders (MAE): Self-Supervised learning by reconstructing missing image patches .................................................................... 600
- Federated learning (Privacy-Preserving Deep Learning) for distributed image classification ... 605
- Adversarial attacks on neural networks for image classification ..................... 610
  - Fast Gradient Sign Method (FGSM) · 611
- Toward building an image search engine with EfficientNet features .................... 615
- Object detection ................................................................. 618
- Object detection with RetinaNet .................................................... 619
- Object detection and instance segmentation with mask R-CNN .......................... 621
- Optical Character Recognition (OCR) and Vision-Language Text Extraction Pipelines ............ 622
  - Baseline OCR with Tesseract • 623
  - Deep Learning OCR: PPOCR (PaddleOCR) • 624
  - ALPR (Automatic License Plate Recognition): End-to-End OCR via detection + recognition • 625
  - Scene text recognition with EasyOCR • 628
  - Transformer-Based OCR: TrOCR • 629
- Augmented reality filters and Landmark-Based Face–Hand analysis with MediaPipe ............ 629
  - Face landmark detection and Snapchat-Style filters • 630
- Hand landmark detection and gesture visualization ................................................ 631
- Deepfake generation with the First Order Motion Model ................................................ 633
- Car damage detection and severity assessment using YOLOv8 segmentation ........................ 634
- Summary ................................................................................................................................ 637

## Chapter 13: Generative AI in Image Processing and Computer Vision 639

- Technical requirements ........................................................................................................ 640
- Class-conditional image generation with Conditional Generative Adversarial Networks (cGANs) ............ 640
- Conditional image generation with Conditional Variational AutoEncoder (CVAE) .................. 648
- Pix2Pix and CycleGAN for image-to-image translation .................................................... 653
- StyleGAN-based high-fidelity face synthesis and semantic manipulation .............................. 657
  - StyleGAN3: Alias-free generative modeling • 657
  - StyleGANEX: High-fidelity inversion and identity editing • 661
- GFPGAN: Blind face restoration using Generative Facial Priors ........................................ 664
- Denoising Diffusion Probabilistic Models (DDPMs) ...................................................... 666
  - Forward diffusion process • 666
  - Reverse diffusion process • 668
  - Latent Diffusion Models • 669
  - Latent space interpolation • 670
  - Understanding latent diffusion • 671
- Text-to-image generation with Stable Diffusion ............................................................ 672
- Image editing with InstructPix2Pix ....................................................................... 674
- Structure-guided image generation with ControlNet ...................................................... 676
- Practical applications of Stable Diffusion and SDXL ...................................................... 677
  - SDXL-Turbo text-to-image generation • 678
  - Typography generation • 679
  - Prompt sensitivity • 679
  - Speed-quality tradeoff • 680
  - Seed exploration • 680
  - Image-to-image refinement with SDXL-Turbo • 681
  - Face restoration • 682
  - Rendering style transformation • 682
  - Diffusion-based super-resolution • 683
  - Image inpainting • 684
  - Artistic style generation • 684
- IP-Adapter and ControlNet for identity-preserving pose-guided generation ................................ 685
- Image generation with DALL·E 2 and DALL·E 3 APIs ........................................................ 687
- GPT image APIs: generation, editing, inpainting, and vision ................................................ 691
  - Image understanding with GPT vision • 692
  - Image editing with GPT image • 693
  - Edge-Guided editing • 693
  - Image inpainting • 694
  - AI style transfer • 695
  - Face restoration • 696
  - Multi-image composition • 697
  - E-Commerce product enhancements • 698
  - Visual Question Answering (VQA) • 699
  - Automated industrial inspection (detecting manufacturing defects) • 699
  - Image colorization • 701
  - Sketch-to-photorealistic synthesis • 702
  - Painting-to-photorealistic conversion • 702
  - Local object editing • 703
  - Object removal • 703
  - Background replacement • 704
  - Scene transformation • 704
  - Change detection in remote sensing • 706
  - Medical image interpretation • 707
  - Generative AI for chemistry • 708
- Summary ................................................................................................................................ 710

## Chapter 14: Unlock Your Exclusive Benefits 711

## Other Books You May Enjoy 716

## Index 719

## Preface

Image processing and computer vision have become foundational technologies in modern artificial intelligence, enabling machines not only to enhance and analyze images but also to interpret, understand, and generate visual content. Today, these techniques are widely used across healthcare, autonomous systems, robotics, remote sensing, scientific imaging, manufacturing, and creative media.

This book presents a hands-on and mathematically grounded journey from the fundamentals of digital image processing to state-of-the-art advances in computer vision and generative AI using Python. It begins with core concepts such as image representation, sampling, quantization, Fourier analysis, convolution, and spatial and frequency-domain filtering. Building on these foundations, it explores image enhancement, derivative-based processing, and classical image restoration techniques for solving real-world imaging problems.

The book then transitions into modern computer vision paradigms, including image segmentation, image classification, and object detection, covering both classical approaches and deep learning-based methods. Readers gain practical experience with widely used architectures and frameworks, along with applications in domains such as medical imaging, remote sensing, industrial inspection, and scene understanding.

The final chapter introduces cutting-edge generative AI methods that are redefining computer vision. Topics include GANs, VAEs, diffusion models, Stable Diffusion, ControlNet, DALL·E, GPT-based image systems, and multimodal vision-language models. These approaches illustrate the evolution of vision systems from passive perception to active generation, editing, restoration, and reasoning over visual data.

A central philosophy of this book is learning from first principles. Each topic is developed through rigorous mathematical foundations, followed by intuitive explanations and practical Python implementations. This integration of theory, intuition, and hands-on coding is designed to help readers understand not only how these methods work but also why they work.

In this second edition, the content has been extensively revised and expanded to reflect rapid advances in deep learning, multimodal AI, and generative modeling. New material incorporates recent state-of-the-art methods while maintaining a strong emphasis on mathematical rigor and practical implementation.
