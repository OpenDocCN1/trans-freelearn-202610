Before applying the core Scrypt function, the algorithm takes P and S as input and applies PBKDF2 and SHA-256-based HMAC. Then the output is fed to an algorithm called ROMix, which internally uses the Blockmix algorithm utilizing the Salsa20/8 core stream cipher to fill up the memory which requires large memory to operate, thus enforcing the sequentially memory hard property.

The output from this step of the algorithm is finally fed to the PBKDF2 function again in order to produce a derived key. This process is shown in the following diagram:

![](bbox=[0.35, 0.29, 0.62, 0.62])

Scrypt is used in litecoin mining with specific parameters where N= 1024, R = 1, P=1, and S= random 80 bytes producing a 256-bit output.

It appears that, due to the selection of these parameters, the development of ASICs for Scrypt for litecoin mining turned out to be not very difficult. In an ASIC for litecoin mining, a sequential logic can be developed that takes the data and nonce as input and applies the PBKDF2 algorithm with HMAC-SHA256; then the resultant bit stream is fed into the SALSA20/8 function which produces a hash that again is fed down to the PBKDF2 and HMAC-256 functions to produce a 256-bit hash output. As is the case with bitcoin PoW, in Scrypt also if the output hash is less that the target hash (already passed as input at the start, stored in memory, and checked with every iteration) then the function terminates; otherwise, the nonce is incremented and the process is repeated again until a hash is found that is lower than the difficulty target: