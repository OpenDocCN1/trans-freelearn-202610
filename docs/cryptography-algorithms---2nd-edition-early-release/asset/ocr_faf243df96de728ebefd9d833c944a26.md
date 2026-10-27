differently by symmetric scheme encryption, so the effective comparison has to be done with asymmetric encryption:

# Comparable Key Sizes for Equivalent Security

| Symmetric scheme (key size in bits) | ECC-based scheme (size of n in bits) | RSA/DSA (modulus size in bits) |
|---:|---:|---:|
| 56 | 112 | 512 |
| 80 | 160 | 1024 |
| 112 | 224 | 2048 |
| 128 | 256 | 3072 |
| 192 | 384 | 7680 |
| 256 | 512 | 15360 |

> _Figure 7.9 – William Stallings' table of comparison – ECC versus classical cryptography_

As you can see, 256-bit encryption performed on ECC is equivalent to a 3,072-bit key on RSA. Moreover, we can compare a 512-bit key on elliptic curve to a key size on RSA of 15,360 bits. Compared with symmetric encryption (AES, for example), ECC needs double the amount of bits.

# Important Note

A warning regarding the key's length: it doesn't matter how long the modulus is or how big the key size is – if an algorithm logically breaks out, nothing will repair its defeat.

# Considerations about the future of ECC

Now that we have seen how a practical attack on ECDSA works, one of the most interesting questions we should ask for the future is the following: Is _elliptic curve cryptography resistant to classical and quantum attacks?_ At a