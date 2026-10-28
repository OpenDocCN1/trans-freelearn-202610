
## 14
## New Frontiers

In the previous chapters, we overviewed many of the tools and applications of network science within analytics projects. In this chapter, we’ll look ahead toward the newer tools being developed that have many promising applications within network science, including quantum graph algorithms, deep learning/large language model architecture optimization, and multilevel graphs that are useful for organizing metadata and understanding genetics data.

While the prior chapters included coded examples, this chapter will focus on ideas and the possibilities for development in the future. Network science is an evolving field, and it’s likely that tools we can’t even imagine right now will be commonplace in the next decade. Let’s dive into some of the newer applications and see how network science continues to contribute to knowledge in many different fields.

Specifically, we will cover the following topics:
- Quantum network science algorithms
- Neural network architectures as graphs
- Hierarchical networks
- Hypergraphs

### Quantum network science algorithms

One new and promising avenue for network science algorithm development is quantum computing. **Quantum computing** leverages many of the advantageous properties of physics at the quantum level to improve computing power and tackle difficult problems. While there are many flavors of quantum computing, we’ll focus on **qubit** systems, where bits are replaced with their quantum version.

Qubits offer many advantages over bits within computing frameworks. **Superposition** is a property in quantum physics that allows particles, such as qubits, to exist in multiple states at the same time. Thus, whereas bits must exist in a 0 or 1 configuration, qubits can exist as a 1 and a 0 simultaneously until the qubit is measured, collapsing to the usual state of a bit. This allows for massively parallel searches for solutions.

In addition to being able to exist as a 1 and a 0 at the same time, superposition allows for fractional values where the qubit exists as partially a 1 and partially a 0 in a probabilistic sense. Because of this fractional state, it’s possible to design gates that work on the probability function rather than acting on single states of a bit. This flexibility allows for circuit designs to enhance algorithms, and such an approach to computing has shown gains in image classification. *Figure 14.1* shows the differences between a qubit and a bit:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_266_0.png)

Bit in 1 or 0 configuration

Qubit in superposed state

Figure 14.1 — A visualization of qubits versus bits

Many types of machine learning algorithms have been translated for use on quantum computers to speed up solution convergence or scale algorithms to larger problems. Quantum machine learning is an active area of research, and network science algorithms are one of the larger areas of research within quantum machine learning. Let’s explore two quantum network science algorithms in more detail.

### Graph coloring algorithms

**Graph coloring algorithms** are a branch of network science algorithms aimed at finding the chromatic number of a graph. A **chromatic number** is the minimum number of colors required to color the vertices of a network such that no colors are adjacent to the same color. In real-world applications, chromatic numbers are important in determining the number of channels to allocate in communication systems or scheduling jobs within parallel processing. The minimum number of colors required to color the vertices of a network represents the minimum number of channels needed or the minimum

number of jobs needed to complete the aforementioned tasks. *Figure 14.2* shows a small network with the colors shown:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_267_0.png)

Figure 14.2 – A small network with four vertices, four edges, and a chromatic number of two

While *Figure 14.2* has an easy-to-determine chromatic number, computing the chromatic number of a network, in general, is thought to be **NP-hard**, meaning that it is not solvable in polynomial time. Algorithms that have high complexity translates to computation that is very difficult or currently impossible for large enough or dense enough networks. This poses problems in terms of solving problems in communication and job scheduling where the volume of calls or jobs is very large.

Fortunately, quantum graph coloring algorithms allow algorithms to run much faster than they do on classical machines, leading to quicker solutions for large graphs and potentially computable solutions for very large graphs. However, current quantum hardware systems limit the size of a graph on which a quantum graph coloring algorithm can be run, as the problem must be mapped to the physical system. As quantum hardware supports larger and larger networks, chromatic number computation will be feasible for more real-world problems.

### Max flow/min cut

In *Chapter 4*, we explored **transportation logistics** and the **max flow/min cut algorithm**. Recall that this algorithm is useful in partitioning flows on networks and that it can be used to plan out road closures on a traffic grid, such as the grid shown in *Figure 14.3*:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_268_0.png)

Figure 14.3 – A traffic grid in relation to five stores (considered in Chapter 4)

Finding the optimal cut to avoid service delays to each of these five stores (shown in Figure 14.3) is a difficult problem to compute, and computationally efficient solutions provide a good way to scale these types of analyses.

Another common use of the max flow/min cut algorithm is image partitioning, whereby the foreground and background of an image are determined. Image enhancement typically relies on distinguishing different parts of focus within the image. Photoshopping images also requires a foreground/background discernment step.

As with chromatic number calculations, computing max flow/min cut optima on large or dense graphs can be challenging when dealing with classical systems. Quantum versions of this algorithm exist and can optimize solutions more easily than classical versions. However, hardware size currently limits the application of quantum max flow/min cut algorithms on large problems.

Many other quantum versions of network algorithms exist and have given strong results on small problems, representing a solid future direction for scaling network algorithms in terms of newer approaches to hardware. As hardware development progresses, it’s likely that network science will benefit, and the algorithms in network science that currently don’t scale as well as other network science algorithms will find a path forward for difficult problems. So far, network science algorithms represent a major part of quantum machine learning research, and given their success, it’s likely that network science and quantum computing will continue to influence each other.

Let’s turn our attention to deep learning architectures, which can be formulated in both classical computing and quantum computing.

### Neural network architectures as graphs

In *Chapter 1*, we touched on deep learning models, particularly in the context of generative artificial intelligence. Deep learning models surface in many areas of analytics, including **natural language processing, image classification,** and **time series forecasting**. Let’s explore these applications in more detail.

Natural language processing is ubiquitous in the era of big data. Surveys often employ free text collection methods. Customers provide text reviews of products. Social scientists jot down notes when they are observing populations qualitatively (dubbed **ethnographic research**). Bloggers post regular content to disseminate ideas. Legal, medical, and educational notes can include biased content that needs to be addressed to protect people from institutional bias that limits future opportunities and wellness.

All this data needs to be parsed before it is fed into a classification model or exploratory data tools. Most of the tools that do this rely on deep learning models that can connect pieces of information separated across a sentence or a paragraph, which are termed **recurrent neural networks**. Many of the common packages in Python rely on pretrained recurrent neural networks, where the training steps have already been performed on a large dataset and saved as a model. Updating the pretrained models often involves adjusting the neural network weights and/or connections based on new data, perhaps from a specific domain of interest such as chemistry or law. *Figure 14.4* shows a simple recurrent neural network architecture. In practice, recurrent neural networks can have many more hidden layers and nodes with connections between them:

![](img/71ff737206f7d80bb625b3c9aca9dc7f_269_0.png)

Figure 14.4 — A simple recurrent neural network diagram, where the hidden layers provide feedback to each other in a forward and backward manner

Another key task in analytics these days is image classification. Social media allows users to upload high volumes of images across a dizzying array of topics that need to be tagged with topic information and flagged if it is not safe content. Trap cameras and drones allow for the surveillance of wildlife and potential poachers in protected areas, and these images need to be analyzed to protect wildlife and divvy up limited resources. Farmers can now monitor crops using robotic imaging technologies, which then match the images to disease classification models for the early identification of disease or distress in crops that endanger local food sources or hurt local economies. *Figure 14.5* shows three
