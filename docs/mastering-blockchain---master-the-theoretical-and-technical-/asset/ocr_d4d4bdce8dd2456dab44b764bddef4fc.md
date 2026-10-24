# # Litecoin

Litecoin is a fork of the bitcoin source code released in 2011. It uses Scrypt as PoW, originally introduced in the Tenebrix coin. Litecoin allows for faster transactions as compared to bitcoin due to its faster block generation time of 2.5 minutes. Also difficulty readjustment is achieved every 3.5 days roughly due to faster block generation time. The total coin supply is 84 million.

Scrypt is a sequentially memory hard function that is the first alternative to the SHA-256-based PoW algorithm. It was originally proposed as a password-based key derivation function $PBKDF$. The key idea is that if the function requires large number of memory to run then custom hardware such as ASICs will require more VLSI area, which would be unfeasible to build. The Scrypt algorithm requires a large array of pseudo random bits to be held in memory and a key is derived from this in a pseudo random fashion. The algorithm is based on a phenomenon called **Time-Memory Tradeoff (TMTO)**. If memory requirements are relaxed then it results in increased computational cost. Put another way, TMTO shortens the running time of a programme if more memory is given to it. This tradeoff makes it infeasible for an attacker to gain more memory because it's expensive and difficult to implement on custom hardware, or if the attacker chooses to not increase memory, then it results in the algorithm running slowly due to high processing requirements.

Scrypt uses the following parameters to generate a derived key (Kd):

+   - Passphrase: This is a string of characters to hash
+   - Salt: This is a random string that is provided to Scrypt functions (generally all hash functions) in order to provide a defence against brute-force dictionary attacks using rainbow tables
+   - N: This is a memory/CPU cost parameter that must be a power of 2 > 1
+   - P: The parallelization parameter
+   - R: The block size parameter
+   - dkLen: The intended length of the derived key in bytes

Formally, this function can be written as follows:

```
$Kd = scrypt$ $(P, S, N, P, R, dkLen)$
```