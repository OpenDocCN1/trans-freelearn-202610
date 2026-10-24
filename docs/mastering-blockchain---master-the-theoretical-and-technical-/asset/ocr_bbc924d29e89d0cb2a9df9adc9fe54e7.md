**Hash functions** are typically used to provide data integrity services. These can be used as one-way functions and to construct other cryptographic primitives, such as MACs and digital signatures. Some applications used hash functions as a means of generating **pseudo random numbers (PRNGs)**. Hash functions do not require a key. There are two practical and three security properties of hash functions that must be met depending on the level of requirements of integrity.

# Compression of arbitrary messages into fixed length digest

This property is concerned with the fact that a hash function must be able to take a long input text of any length and output a fixed length compressed message. Hash functions produce a compressed output in various bit sizes, usually between 128-bits and 512-bits.

# Easy to compute

Hash functions are efficient and fast one-way functions. The requirement is that they be very quick to compute regardless of the message size. The efficiency may decrease if the message is too big but the function should still be fast enough for practical use.

In the following section, security properties of hash functions are discussed.

# Pre-image resistance

Consider an equation:

```
$$h(x)=y$$
```

Here, *h* is the hash function, *x* is the input, and *y* is the hash. The first security property requires that *y* cannot be reverse computed to *x*. *x* is considered a *pre-image* of *y*, hence the name pre-image resistance. This is also called one-way property.

# Second pre-image resistance

This property requires that given *x* and *h(x)*, it is almost impossible to find any other message *m*, where *m != x* and hash of *m* = hash of *x*. *h(m) = h(x)*. This property is also known as weak collision resistance.