
is a discrete power, but the problems of searching for discrete logarithm and discrete power have the same computational difficulty.

Let's go a little bit deeper now to explain the dynamics of this operation. Let's consider computing the following: 2^4 ≡ (x) (mod 13) First, we calculate 2^4 = 16, then divide 16:13 = 1 by the remainder of 3 so that x = 3. The discrete logarithm is just the inverse operation: 2^[y] ≡ (x) (mod 13) In this case, we have to calculate [y] while knowing the base is 2. It's possible to demonstrate that there are infinite solutions for [y] that generate (x). Taking the preceding example, we have the following: 2^[y] ≡ 3 (mod 13) for [y] One solution is y = 4, but it is not the only one. The result of 3 is also satisfied for all the integers, (n), of this equation: [y] = [y + (p-1)*n] Let's prove n = 1: 2^[4+(13-1)*1] ≡ 2^16 (mod 13) 2^16 ≡ 3 (mod 13) But it is also valid for n = 2: 2^[4 +(13-1)*2] ≡ 2^28 (mod 13) 2^28 ≡ 3 (mod 13) And so on... Hence, the equation has infinite solutions for all the integers; that is, (n ≥ 0): [y] ≡ 2^[4 + 12 n] (mod 13) There is no method yet for solving the discrete logarithm in polynomial time. So, in mathematics, as in cryptography, this problem is considered very hard to solve, even if the attacker has a lot of computation power. Finally, we have to introduce the definition and the notation of a generator, (g), which is a particular number where we say that (g) generates the entire group, (Zp). If (p) is a prime number, this means that (g) can take on any value between 1 and p-1.

## Explaining the D-H algorithm

D-H is not exactly an asymmetric encryption algorithm, but it can be defined properly as a public/private key algorithm. The difference between asymmetric and public/private keys is not only formal but substantial. You will understand the difference better later, in the RSA section, which covers a pure asymmetric algorithm. Instead, D-H gets a shared key, which works to symmetrically encrypt the message, [M]. The encryption that's performed with a symmetric algorithm is combined with a D-H shared key transmission to generate the cryptogram, C: Symmetric-Algorithm E((D-H[k]), M) = C In other words, we use the

D-H algorithm to generate the shared secret key, [k], then with AES or another symmetric algorithm, we encrypt the message, [M]. D-H doesn't directly encrypt the secret message; it can only determine a shared key between two parties. This is a critical point, as we will see in the next paragraph. However, for working in discrete fields and applying a discrete logarithm problem to shield the key from attackers when sharing it, Diffie and Hellman implemented one of the most robust and famous algorithms in cryptography. Let's see how D-H works:

- **Step 1:** Alice and Bob first agree on the parameters: (g) as a generator in the ring, (Zp), and a prime number, p (mod p).
- **Step 2:** Alice chooses a secret number, [a], and Bob chooses a secret number, [b].

Alice calculates **A ≡ g^a (mod p)** and Bob calculates **B ≡ g^b (mod p)**.

- **Step 3:** Alice sends (A) to Bob and Bob sends (B) to Alice.
- **Step 4:** Alice computes **ka ≡ B^a (mod p)** and Bob computes **kb ≡ A^b (mod p)**.

**[ka = kb]** will be the secret that's shared key between Alice and Bob. The following is a numerical example of this algorithm:

- **Step 1:** Alice and Bob agree on the parameters they will use; that is, g = 7 and (mod 11).
- **Step 2:** Alice chooses a secret number, (3), and Bob chooses a secret number, (6).

Alice calculates **7^3 (mod 11) ≡ 2** and Bob calculates **7^6 (mod 11) ≡ 4.**

- **Step 3:** Alice sends (2) to Bob and Bob sends (4) to Alice.
- **Step 4:** Alice computes **4^3 (mod 11) ≡ 9** and Bob computes **2^6 (mod 11) ≡ 9.**

The number **[9]** is the shared secret key, **[k]**, of Alice and Bob.

## Analyzing the algorithm

In *Step 2* of the algorithm, Alice calculates **A = g^a (mod p)** and Bob calculates **B = g^b (mod p)**. Alice and Bob have exchanged **(A)** and **(B)**, the public parameters of the one-way function. A *one-way function* has this name because it is impossible, **(-x%)**, to return from the public parameter, **(A)**, to calculate the secret private key, **[a]** (and the same for **(B)** with **[b]**), for the robustness of the discrete logarithm (see the *The discrete logarithm* section). Another property of the modular powers is that we can write **B^a** and **A^b (mod p)**, as follows: **B^a ≡ (g^b)^a (mod p)** **A^b ≡ (g^a)^b (mod p)** So, for the property of modular exponentiations, we have the following: **g^(b*a) ≡ g^(a*b) (mod p)** For example, we have the following:

- Alice: **(7^6)^3 ≡ 7^(6*3) ≡ 9 (mod 11)**
- Bob: **(7^3)^6 ≡ 7^(3*6) ≡ 9 (mod 11)**

This is the mathematical trick that makes it possible for the D-H algorithm to work. Now that we have understood the algorithm, let’s highlight its defects and the possible attacks that can be performed on it.

## Possible attacks and cryptanalysis on the D-H algorithm

The most common attack that's performed on the D-H algorithm is the **man-in-the-middle (MiM)** attack. A MiM attack is when the attacker infiltrates a channel of communication and spies on it, blocks, or alters the communication between the sender and the receiver. Usually, the attack is accomplished by Eve (the attacker) pretending to be one of the two true actors in the conversation:

![](img/2c67eb1b2bfb299c02a010d62900759c_99_0.png)

*Figure 3.2 – Eve is the “man in the middle”*

Recalling what happened in *Step 3* of the D-H algorithm, Alice and Bob exchanged their public parameters, **(A)** and **(B)**. Here, Alice sends **(A)** to Bob, then Bob sends **(B)** to Alice. Now, Eve (the attacker) interferes within the communication by pretending to be Alice. So, a MiM attack looks like this:

- **Step 3:** Alice sends **(A)** to Bob and Bob sends **(B)** to Alice.
- **Here, we have the MiM attack:** Eve sends **(E)** to Bob and then Bob sends **(B)** to Eve, assuming it is Alice.

Let's analyze Alice's function, **A ≡ g^a (mod p)** and Bob's function **B ≡ g^b (mod p)**. This passage would be crucial if it was done in normal arithmetic and not in finite fields. There is another possible attack, also known as the **birthday attack**, which is one of the most famous attacks performed on discrete logarithms. It is consolidated that among a group of people, at least two of them will share a birthday so that in a cyclic group, it will be possible to find some equal values (collisions) to solve a discrete logarithm.

## Important Note

**(E)** here represents Eve's public parameter, which has been generated by her private key, not encryption.

equal to its logical simplicity and hidden inside is such a force that after 40 years, it's still used to protect more than 80 percent of commercial transactions in the world. Its acronym is made up of the names of its three inventors – **Rivest, Shamir, and Aldemar.** **RSA** is what we call the perfect asymmetric algorithm. Actually, in 1997, the CESG, an English cryptography agency, attributed the invention of public-key encryption to James Allis in 1970 and the same agency declared that in 1973, a document was written by Clifford Cocks that demonstrates a similar version of the RSA algorithm. The essential concept of the asymmetric algorithm is that the keys for encryption and decryption are different. Recalling the analogy to padlocks I made in the *The Diffie-Hellman algorithm* section, when I described the D-H algorithm, we saw that anybody (not just Alice and Bob) could lock the box with a padlock. This is the true problem of MiM because the padlock can't be recognized as specifically belonging to Bob or Alice. To overcome this problem, another interesting mental experiment can be done using a similar analogy but making things a little different. Suppose that Alice makes many copies of her padlock, and she sends these copies to every postal office in the country, keeping the key that opens all the padlocks in a secret place. If Bob wishes to send a secret message to Alice, he can go to a postal office and ask for Alice's padlock, put the message inside the box, and lock it. In this way, Bob (the sender), from the moment he locks the box, will be unable to unlock it, but when Alice receives the box, she will be able to open it with her unique secure key. Basically, in RSA (as opposed to D-H), Bob encrypts the message with Alice's public key. After the encryption process, even Bob is unable to decrypt the message, while Alice can decrypt it using her private key. This was the step that transformed the concept of asymmetric encryption from mere theory to practical use. RSA discovered how to encrypt a message with the public key of the receiver and decrypt it with the private key. To make that possible, RSA needs a particular mathematic function that I will explain further later on when we explore the algorithm in detail. As we mentioned previously, there were three inventors of this algorithm. They were all researchers at MIT, Boston, at the time. We are talking about the late 1970s. After the invention of the D-H algorithm, Ronald Rivest was extremely fascinated by this new kind of cryptography. He first involved a mathematician, Leonard Adleman, and then another colleague from the Computer Science department, Aid Shamir, who joined the group. What Rivest was trying to achieve was a mathematical way to send a secret
