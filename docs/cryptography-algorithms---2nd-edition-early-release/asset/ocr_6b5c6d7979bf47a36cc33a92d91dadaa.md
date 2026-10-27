# Figure 2.19 – The key generation scheme

Because of this compression/confusion/permutation technique, DES is able to determine different sub-keys, one per round of 48 bits. This makes DES difficult to crack.

# Encryption

After we have generated the key, we can proceed with the encryption of the message, [M]. The encryption scheme of DES consists of three phases:

+   - **Step 1 – initial permutation:** First, the bits of the message, [M], are permutated through a function that we call **Initial Permutation (IP)**. This operation from a cryptographic point of view does not seem to augment the security of the algorithm. After the permutation, the 64 bits are divided into 32 bits in **L0** and 32 bits in **R0** just like we did in simplified DES.

+   - **Step 2 – rounds of encryption:** For **0 ≤ i ≤ 16**, the following operations are executed:

Li = Ri-1 Compute **Ri = Li-1 f(Ri-1, Ki)**. Here, **Ki** is a string of 48 bits obtained from the key **K** (round key j) and **f** is a function of expansion similar to the **f** described earlier for simple DES. Basically, for **i = 1** (the first round), we have the following: L1 = R0 R1 = L0s f(R0, K1)

+   - **Step 3 – final permutation:** The last part of the algorithm at the 16th round (the last one) consists of the following:

+   - Exchanging the left part, **L16**, with the right part, **R16**, in order to obtain (**R16, L16**)

+   - Applying the inverse, **INV**, of the IP to obtain the ciphertext, **c**, where **c = INV(IP (R16, L16))**

The following figure is a representation of an intelligible scheme of DES encryption: