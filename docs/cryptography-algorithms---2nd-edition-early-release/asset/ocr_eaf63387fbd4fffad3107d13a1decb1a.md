*confusion* on the bit positions; then, the key is divided into two 28-bit sub-keys called **C0** and **D0**. After this operation (always in the same line to create a bit of confusion and diffusion), it performs a circular shift process as shown in the following table:

| Round | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Number of bits shifted | 1 | 1 | 2 | 2 | 2 | 2 | 2 | 2 | 1 | 2 | 2 | 2 | 2 | 2 | 2 | 1 |

*Figure 2.17 – Showing the number of key bits shifted in each round in DES*

If you look at the preceding table, you can notice rounds 1, 2, 9, and 16 shift left by only 1 bit; all the other rounds shift left by 2 bits. Let's take as an example of **C0, D0** (the original division of the key in 28-bit left and 28-bit right), expressed in binary notation as follows:

C0 = 1111000011001100101010101001   D0 = 01010101011001100111100011

from **C0** and **D0**, **C1** and **D1** will be generated, as follows:

C1 = 1110000110011001010101010011   D1 = 10101010110011001111000111

you focus on the step of the generation of **C0 --> C1**, you can better understand how it works – it's a simple shift to the left of all the bits of C0 with respect to C1:

C0 = 1111000011001100101010101001C1 = 1110000110011001010101010011

the circular shift, as explained, the next step is to process a selection of 48 bits over the subset key of 56 bits. It's a simple permutation of position: just to give an example, bit number 14 moves to the first position and bit number 32 moves to the last position (48th). As you can see in the following table, some bits, just like bit number 18, are discarded in the new configuration, so you don't find them in the table. At the end of the process of bit compression, only 48 bits are selected; consequently, 8 bits are discarded:

| 14 | 17 | 11 | 24 | 1 | 5 | 3 | 28 | 15 | 6 | 21 | 10 |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 23 | 19 | 12 | 4 | 26 | 8 | 16 | 7 | 27 | 20 | 13 | 2 |
| 41 | 52 | 31 | 37 | 47 | 55 | 30 | 40 | 51 | 45 | 33 | 48 |
| 44 | 49 | 39 | 56 | 34 | 53 | 46 | 42 | 50 | 36 | 29 | 32 |