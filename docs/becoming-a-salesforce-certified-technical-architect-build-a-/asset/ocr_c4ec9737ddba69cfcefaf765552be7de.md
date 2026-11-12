The following diagram explains the general mechanism of a hashing algorithm:

![](bbox=[0.129, 0.14, 0.844, 0.479])

# Figure 3.6 – A diagram of the way hashing algorithms work

# MAC Algorithms

MAC algorithms are similar to hashing algorithms in that both generate a one-way digest/hash of the plaintext. However, MAC algorithms are mainly created to _verify the authenticity of the message sender_, in addition to verifying that the message itself has not been tampered with.

To do that, MAC algorithms use a key in the process. Typically, this is used to encrypt the hashed value itself. The key should be known to both the sender and the receiver. It can be used to verify that the entity that generated the hash owns a copy of the same key, which is proof of the authenticity of the message itself.

Examples of popular modern MAC algorithms include the **keyed-hash message authentication code (HMAC)** and its different versions, that is, HMACMD5, HMACSHA1, HMACSHA256, and HMACSHA512. All these versions are supported by the Salesforce `Crypto` class.