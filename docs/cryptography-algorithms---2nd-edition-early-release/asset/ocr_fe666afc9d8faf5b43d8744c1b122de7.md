![](bbox=[0.13, 0.11, 0.88, 0.26])

Figure 4.6 – Scheme of the shift right bit operation (1 position)

As before, let's consider this for the decimal number 23, which is 00010111 in byte notation. If we do a shift right bit operation, we will get the decimal number 11 as the result, as you can see in the following example:

![](bbox=[0.12, 0.4, 0.88, 0.66])

Figure 4.7 – Example of the shift right bit operation (1 position)

+   - Xr: This is left bit rotation. This operator (also represented as <<<) means a circular rotation of bits, similar to shift, but with the key difference here that the initial part becomes the final part. It's used in the SHA-1 algorithm to rotate the variables A and B, respectively, by 5 and 30 positions (A<<<5; B<<<30) as will be further explained in the following section.