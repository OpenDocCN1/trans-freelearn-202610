D-H algorithm to generate the shared secret key, [k], then with AES or another symmetric algorithm, we encrypt the message, [M].D-H doesn't directly encrypt the secret message; it can only determine a shared key between two parties. This is a critical point, as we will see in the next paragraph.However, for working in discrete fields and applying a discrete logarithm problem to shield the key from attackers when sharing it, Diffie and Hellman implemented one of the most robust and famous algorithms in cryptography.Let's see how D-H works:

+   - **Step 1:** Alice and Bob first agree on the parameters: (g) as a generator in the ring, (Zp), and a prime number, p (mod p).
+   - **Step 2:** Alice chooses a secret number, [a], and Bob chooses a secret number, [b].

Alice calculates **A ≡ g^a (mod p)** and Bob calculates **B ≡ g^b (mod p)**.

+   - **Step 3:** Alice sends (A) to Bob and Bob sends (B) to Alice.
+   - **Step 4:** Alice computes **ka ≡ B^a (mod p)** and Bob computes **kb ≡ A^b (mod p)**.

**[ka = kb]** will be the secret that's shared key between Alice and Bob.The following is a numerical example of this algorithm:

+   - **Step 1:** Alice and Bob agree on the parameters they will use; that is, g = 7 and (mod 11).
+   - **Step 2:** Alice chooses a secret number, (3), and Bob chooses a secret number, (6).

Alice calculates **7^3 (mod 11) ≡ 2** and Bob calculates **7^6 (mod 11) ≡ 4.**

+   - **Step 3:** Alice sends (2) to Bob and Bob sends (4) to Alice.
+   - **Step 4:** Alice computes **4^3 (mod 11) ≡ 9** and Bob computes **2^6 (mod 11) ≡ 9.**

The number **[9]** is the shared secret key, **[k]**, of Alice and Bob.

# Analyzing the algorithm