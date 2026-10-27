
- Eve (the attacker) picks up a random number, **[r]**, and calculates the following:

**[r] \* (v1) ≡ e (mod N)**

**[r]** and **(e)** are public, so that is known by everyone. By means of this function, Eve can extract **(v1)**.

- In parallel, Eve calculates the following:

**e \* x ≡ c (mod N)**

The parameters **(e, c)** are also known. This, just like in step 1, is an inverse multiplication (modulo **N**). The scope of this operation is to obtain **[x]**. Then, using **[x]**, Eve multiplies **[x]** by **[r]**, yielding **(v2)**:

**x \* r ≡ v2 (mod N)**

Eve sends **(v1, v2)** to Victor, who can verify the following:

**v1 \* v2 ≡ c (mod N)**

Eve can impersonate Peggy, and she can claim to know **[m]** even if she doesn't know it!

**Numerical example:** **r = 39** is the secret number chosen by Eve. (**N**, **c**, **e**) are the same public parameters of the previous example (**N = 2430101**, **c = 160613**, **e = 9007**).

- **Step 1:** Eve calculates **v1**:

**e \* r^-1 ≡ v1 (mod N)**

**9007 \* 436172 = 1557988 (mod 2430101)**

**v1 = 1557988**

- **Step 2:** Eve performs **v2**.

The next operation is to solve **e \* x ≡ c (mod N)**, obtaining **(x)** in inverse modular multiplication:

**9007 \* x = 160613 (mod 2430101)**

**x = 2031892**

Then, using **x**, Eve obtains **v2**, performing the following operation: **x \* r ≡ v2 (mod N)**, obtaining **v2**:

**v2 = 1480556**

After having gained **v2**, Eve sends (**v1 = 1557988**; **v2 = 1480556**) to Victor.

- **Step 3:** Verification stage.

Finally, Victor can verify the following:

**v1 \* v2 ≡ c (mod N)**

**1557988 \* 1480556 = 160613 (mod 2430101)**

The attack was successful!

### Important Note

Peggy herself could be the primary actor of this trick if she doesn't know **[m]**, but she wishes to convince Victor of it.

This attack works because **(c)** contains **[m]**, and I don't need to demonstrate by showing the value of **[m]**. This protocol isn't required to show **[m]** or its hash, **[H(m)]**, because Peggy doesn't want to reveal any information about **[m]** to Victor. Remember this is *not* an authentication protocol, but it's a proof of statement (or knowledge) that Peggy knows **[m]**. For completeness, we have to add that **(c)** is not a product of 2 big prime numbers (such as **N** is) as probably you have already noticed in the example. So, the logical base of this protocol is weak: finding 2 numbers whose product is **(c)** could be not so difficult even if **(c)** is a large number. To use a hypothetical example, you can think of a scenario where there are two countries: **(A)** has to demonstrate to **(B)** that it holds the formula for an atomic bomb. Using this ZKP, **(A)** could claim to know **[m]** (the formula of the atomic bomb) without really knowing it. This attack could be avoided under certain conditions, one of which is the following:

If Victor already knows **[m]**, then he can require Peggy to send him a hash of the message, **H[m]**. Victor can then verify whether (**v1** and **v2**) are the correct values, and he will accept or deny the verification based on the correspondence of the hash value with **[m]**. In this case, the problem is that the aim of this protocol was not to prove something that was already known but to prove something independently, regardless of whether or not it was known. This last point is very important because if Victor knows **[m]**, then this protocol works; if Victor doesn't know **[m]**, this protocol fails. To avoid these problems, we have to switch to an *interactive* protocol, as we will see in the next section.

## Schnorr's interactive ZKP

The protocol that we saw in the previous section is a non-interactive protocol, where Peggy and Victor don't interact with each other but there is simply a *commitment* between them. The commitment is that Peggy has to show Victor that she knew the message **[m]** without revealing anything about it. Thus, she tries to demonstrate to Victor that she can overcome the RSA problem (or another hard mathematical problem) as proof of her honesty. However, we have also seen that this protocol can be *bypassed* using a mathematical trick. Let's see whether the following interactive ZKP is more robust and can prevent possibly devastating attacks. We always have Peggy and Victor as our main actors. So, let's assume the following:

- **p** is a big prime number.
- **g** is the generator of (**Zp**).
- **B ≡ g^a (mod p)** is the public parameter of Peggy.
- (**p**, **g**, **B**) are public parameters.
- **[a]** is the secret number object of the commitment.

Peggy claims that she knows **[a]**; let’s say that **[a]** is the password to unlock a certain amount of money in a wallet. In order to demonstrate the claim, Peggy and Victor apply the following interactive protocol:

- **Step 1:** Peggy chooses a random integer, **[k]**, where **1 ≤ k < p-1**.

She performs the following calculation: **V ≡ g^k (mod p)**. Peggy sends **(V)** to Victor.

- **Step 2:** Victor chooses a random integer, **(r)**, where **1 ≤ r < p-1**.

Victor sends **(r)** to Peggy.

- **Step 3:** Peggy calculates as follows:

**w ≡ (k - a \* r) (mod p-1)**. Peggy sends **(w)** to Victor.

- **Step 4:** Finally, Victor verifies as follows:

**V ≡ g^w \* B^r (mod p)**. If that is true, Victor should be convinced that Peggy knows **[a]**.

Let's see why the protocol should work and the reason why the last function **(V)** states that Peggy really knows **[a]** (the commitment). First of all, I will show why the protocol is mathematically true, and then I will give a numerical example of this protocol.

## A demonstration of an interactive ZKP

Recall the following instructions:

**V ≡ g^k (mod p)**

**B ≡ g^a (mod p)**

**w ≡ k - a \* r (mod p-1)**

Now we substitute all the past equations inside the last verification of step.

execution of the smart contract (for Peggy) and the payment received (for Victor) are completed successfully. However, many details inherent to the smart contract will not be revealed. So, the role that zk-SNARKs play is fundamental to covering these secrets and executing smart contracts. In order to work, the protocol has to be fast, secure, and easy to implement. As we have already seen, you will notice that this is just what the purpose of a ZKP is – to ease the navigation of an untrustworthy environment. Here, we are talking about blockchains and virtual payments, but essentially the process is similar. So, in this environment, zk-SNARKs keep secrets by protecting the steps involved in a smart contract and, at the same time, proving that all these steps have been executed. This way, they protect the privacy of people and companies. Remember that – not because you have to be super-skeptical, but because you should be realistic – this statement is true under determinate conditions, which I will try to explain as follows:

- The proof given by the prover holds the same computational degree of difficulty as the underlying algorithm chosen as a proof of knowledge.
- There is no mathematical way to trick the verifier with a shortcut or fake proof (such as substituting fake parameters into the **V ≡ g^w \* B^r (mod p)** equation in order to avoid knowing **[a]**).

So, let's see how a zk-SNARK works.

## Understanding how a zk-SNARK works

In this section, first of all, I will try to synthesize how zk-SNARKs generally work, and then we will return with a zk-SNARK protocol related to a proof of knowledge based on a discrete logarithm. As we already have seen for the other ZKPs, a zk-SNARK is composed of three parts or items – (G), (P), and (V):

- **G:** This is a generator of keys, made by a private parameter (the statement or another random key) that generates public parameters given by private keys.
- **P:** This is a proof algorithm that states what the prover wants to demonstrate.
- **V:** This is a verification algorithm that returns a TRUE or FALSE.
