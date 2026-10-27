
To my wife, Mosele Tsemane, for her outstanding support and words of encouragement throughout the writing of this book. To our daughter, Puleng Senekane.

## About the reviewer

**Srinjoy Ganguly** is the founder and CEO of AdroitERA, an EdTech firm, and possesses double masters in Quantum Computing Technology from Technical University of Madrid, Spain and Artificial Intelligence from University of Southampton, UK, respectively. He has over 4 years of experience in Quantum Computing and 5 years of experience in Machine Learning, Deep Learning, AI. He is currently leading Quantum Machine Learning (QML) study space at QWorld and has authored a book on Quantum Computing with Silq Programming. He has given expert talk on QML at IEEE SPS. His research interests include Quantum Machine Learning, Quantum Computing, Quantum Image Processing, Machine Learning and Deep Learning.

## Packt is searching for authors like you

If you're interested in becoming an author for Packt, please visit `authors.packtpub.com` and apply today. We have worked with thousands of developers and tech professionals, just like you, to help them share their insight with the global tech community. You can make a general application, apply for a specific hot topic that we are recruiting an author for, or submit your own idea.

## Table of Contents

## Preface

## Section 1: Fundamentals of Quantum Information Processing

## 1 Getting Started with Quantum Information Processing

- Providing an overview of QIP 4
- Subfields of QIP 9
- Understanding the rationale for choosing Python for QIP 11
- Installing Python and other packages 12
- pipenv and creating the environment 14
- Summary 15
- Further reading 15

## 2 Quantum States, Operations, and Measurements

- Technical requirements 18
- An introduction to linear algebra 18
- Exploring vector spaces 21
- Understanding the properties of a vector 22
- Understanding the properties and types of matrices 24
- Understanding the properties of a tensor product 26
- Exploring the history of quantum information processing 28
- Understanding the qubit 31
- Qubit operations 32
- Quantum measurement 36
- Types of measurement 37
- Dealing with multiple qubits 39

- Understanding the GHZ game 150
- Summary 153
- Understanding the XOR game 152
- Further reading 153

## 7 Quantum Cryptography

- Technical requirements 156
- A history of quantum cryptography 168
- Introducing classical cryptography 156
- Quantum cryptography primitives 168
- Quantum key distribution protocols 171
- A history of classical cryptography 158
- Post-quantum cryptography 179
- Caesar's cipher 158
- The NewHope key exchange scheme 179
- The one-time pad 160
- The SPHINCS+ digital signature scheme 180
- A history of modern cryptography 160
- Summary 182
- Diffie-Hellman key exchange protocol 161
- Further reading 182
- Cryptographic primitives 163
- Quantum cryptography 168

## 8 Quantum Machine Learning

- Technical requirements 184
- Understanding quantum machine learning 191
- Understanding conventional (classical) machine learning 184
- Data encoding 193
- Quantum SVMs 193
- The three main categories of machine learning 185
- Quantum variational classifier 196
- Exploring artificial neural networks 187
- Summary 198
- Further reading 198
- Exploring SVMs 190

## 9 Continuous-Variable Quantum Information Processing

- Technical requirements 200
- Understanding the theory of continuous-variable quantum systems 201
- Introducing continuous-variable quantum information processing 200
- Exploring continuous-variable quantum teleportation 202
- Understanding continuous-variable quantum game theory 205
- Continuous-variable quantum key distribution 206
- Understanding continuous-variable quantum machine learning 207
- Summary 211
- Further reading 211

## 10 Current Trends in Quantum Information Processing

- Exploring current trends in quantum cryptography 214
- Exploring current trends in quantum communication 215
- Understanding current trends in quantum algorithm design 216
- Exploring current trends in quantum machine learning 216
- Understanding current trends in quantum computing hardware technologies 217
- Exploring the future prospects of QIP 218
- Summary 218
- Further reading 219

## Other Books You May Enjoy

## Index

## Preface

Quantum information processing is a field of study that is a subclass of information processing. It exploits the laws of quantum mechanics to enable information processing in a manner that offers some advantages over conventional, non-quantum information processing. Quantum information processing has various sub-fields. These include quantum communication, quantum cryptography, quantum computing, and quantum error correction. This book provides a hands-on introduction to quantum information processing using the Python programming language.

In this book, you will find step-by-step explanations of essential concepts, practical examples, and self-assessment questions. You will begin by exploring the introductory notions of quantum information, including an overview of the prerequisite mathematical tools. Furthermore, you will learn how to use the Python programming language to implement some of the quantum information processing concepts. Finally, you will learn how to use various Python-based quantum information processing frameworks, such as QuTiP, Qiskit, Cirq, Strawberry Fields, and PennyLane.

By the end of this Hands-On Quantum Information Processing with Python book, you will have a deeper understanding and appreciation of the subject of quantum information and how various quantum information processing concepts can be implemented using Python.

## Who this book is for

This book is for software developers, physicists, and mathematicians who are interested in quantum information processing. An understanding of the Python programming language would be beneficial. Furthermore, a basic understanding of mathematics, especially linear algebra, would be beneficial.

## 智谱AI开放平台

智谱AI开放平台（BigModel.cn）是北京智谱华章科技有限公司推出的大模型开放平台，作为智谱AI的官方API入口，为开发者提供弹性的、普惠的开箱即用的大模型服务。

开放平台已汇聚25个系列模型、超过100个模型版本，覆盖语言、图像、视频、音频等模态，提供API、SDK、私有化部署等多种服务方式。

## 全模型统一价格

新用户赠送2000万tokens，低至6.5折优惠，所有模型统一价格计算。

| 模型 | 输入价格(元/百万tokens) | 输出价格(元/百万tokens) |
|------|------------------------|------------------------|
| GLM-4-Plus | 50 | 50 |
| GLM-4-Long | 20 | 20 |
| GLM-4-Air | 1 | 3 |
| GLM-4-Flash | 免费 | 免费 |
| GLM-4V-Plus | 10 | 10 |
| GLM-4V-Flash | 免费 | 免费 |
| GLM-4V | 20 | 20 |
| CogView-3-Flash | 6 | 6 |
| CogVideoX-Flash | 免费 | 免费 |
| Embedding-3 | 0.5 | 0.5 |

计算转化：1汉字≈1token，1英文单词≈1.3token，1英文字符≈0.3token

> 注：以上价格仅供参考，实际价格以官网最新公布为准。

*Chapter 10, Current Trends in Quantum Information Processing,* concludes the book and covers current trends in the field of quantum information processing. It explores current trends in various sub-fields of quantum information processing. Finally, this chapter explores the future of quantum information processing.

## To get the most out of this book

| Software/hardware covered in the book | OS requirements |
|---|---|
| QuTiP | Windows, macOS X, or Linux (any) |
| Qiskit | Windows, macOS X, or Linux (any) |
| Cirq | Windows, macOS X, or Linux (any) |
| Strawberry Fields | Windows, macOS X, or Linux (any) |
| PennyLane | Windows, macOS X, or Linux (any) |

If you are using the digital version of this book, we advise you to type the code yourself or access the code via the GitHub repository (link available in the next section). Doing so will help you avoid any potential errors related to the copying and pasting of code.

## Download the example code files

You can download the example code files for this book from GitHub at https://github.com/PacktPublishing/Hands-On-Quantum-Information-Processing-with-Python. In case there's an update to the code, it will be updated on the existing GitHub repository.

We also have other code bundles from our rich catalog of books and videos available at https://github.com/PacktPublishing/. Check them out!

## Download the color images

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. You can download it here: https://static.packt-cdn.com/downloads/9781800201156_ColorImages.pdf.
