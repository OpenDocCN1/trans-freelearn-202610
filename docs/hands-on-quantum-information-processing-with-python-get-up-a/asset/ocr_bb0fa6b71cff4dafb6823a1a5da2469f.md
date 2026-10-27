# Introducing quantum Fourier transform and quantum phase estimation

As already stated previously, the quantum techniques to be discussed in this section play a crucial role in the quantum algorithms to be discussed later in this chapter. Therefore, it is imperative to cover the following techniques in this section:

+   - QFT
+   - Quantum phase estimation

First, let's introduce and explore QFT.

# Quantum Fourier transform (QFT)

It has been discovered that QFT can be computed faster on a quantum computer than on a classical computer. QFT is the quantum analog of the discrete Fourier transform. We shall recall a discrete Fourier transform that maps the input vector

$$x = \sum_{i=0}^{N-1} x_i$$

into the output vector

$$y_k = \frac{1}{\sqrt{N}} \sum_{j=0}^{N-1} x_j w_N^{jk};$$

where

$$w_N^{jk} = e^{2\pi i \frac{jk}{N}}$$

As already said, QFT is the quantum analog of the discrete Fourier transform discussed previously. Therefore, QFT transforms the input quantum system |x> into the output quantum system |y> such that the following applies:

$$|x\rangle \rightarrow \frac{1}{\sqrt{N}} \sum_{y=0}^{N-1} W_N^{xy} |y\rangle$$