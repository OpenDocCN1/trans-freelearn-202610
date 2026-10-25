# # Understanding the hash function and the Merkle tree

Cryptography is the art and science of hiding a message. It is more of an art than a science. Science here just acts as a mere tool to transform an artistic imagination into a mathematical algorithm.

In this section, we will concentrate only on two specific concepts from this immensely vast subject, which will help us to understand the true essence of a blockchain. These are the hash function and Merkle tree. Please note that these are used for verifying integrity, rather than hiding anything.

Let's now understand what a hash function is and how it works. *Figure 1.3* illustrates a hash function. As we can see in this figure, a hash function is a one-way mapping:

![](bbox=[0.211, 0.548, 0.79, 0.738])

That means that it can take an input (the input is usually a large sequence of bits; it can be a movie, a song, an e-book, a picture, or any digital data) and produce a fixed-size value as output, often much smaller than the input size. However, if I change only one bit in this input, the output will be completely different. This is property number one of the hash function