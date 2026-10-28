In addition to being able to exist as a 1 and a 0 at the same time, superposition allows for fractional values where the qubit exists as partially a 1 and partially a 0 in a probabilistic sense. Because of this fractional state, it’s possible to design gates that work on the probability function rather than acting on single states of a bit. This flexibility allows for circuit designs to enhance algorithms, and such an approach to computing has shown gains in image classification. *Figure 14.1* shows the differences between a qubit and a bit:

![](bbox=[0.285, 0.221, 0.696, 0.581])

Bit in 1 or 0 configuration

Qubit in superposed state

Figure 14.1 — A visualization of qubits versus bits

Many types of machine learning algorithms have been translated for use on quantum computers to speed up solution convergence or scale algorithms to larger problems. Quantum machine learning is an active area of research, and network science algorithms are one of the larger areas of research within quantum machine learning. Let’s explore two quantum network science algorithms in more detail.

# Graph coloring algorithms

**Graph coloring algorithms** are a branch of network science algorithms aimed at finding the chromatic number of a graph. A **chromatic number** is the minimum number of colors required to color the vertices of a network such that no colors are adjacent to the same color. In real-world applications, chromatic numbers are important in determining the number of channels to allocate in communication systems or scheduling jobs within parallel processing. The minimum number of colors required to color the vertices of a network represents the minimum number of channels needed or the minimum