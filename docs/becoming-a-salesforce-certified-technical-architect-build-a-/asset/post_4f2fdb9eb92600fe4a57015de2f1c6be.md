

There are also derived symmetric cryptography algorithms, which operate slightly differently than the algorithms you just covered. While the likes of AES aim to encrypt the plaintext to generate ciphertext, hashing algorithms are designed to ensure the integrity of a particular message. MAC algorithms are used to provide this hashing functionality, in addition to some sort of authentication. The next section covers this in more detail.

## Hashing Algorithms

Hashing algorithms are algorithms that are created to solve the simple problem, How can a recipient ensure that a received message has not been changed or tampered with? Hashing algorithms create a digest out of plaintext. The digest (also known as a hash value) cannot be changed back into plaintext. Hashing algorithms are one-way functions; that is, their output cannot be inverted. Hashing algorithms have the following properties:

- These algorithms are deterministic. So, for the same input message, you always get the same output result (unless you use a salt value, as discussed earlier. Adding a different salt value to the message prior to hashing will generate a different hashed result).
- Due to their nature, it is not feasible for the attacker to try to generate a message that yields a given hash.
- Even a small change to the original plaintext should significantly change the hash value so that the new hash value appears completely unrelated to the old value. This is sometimes referred to as the avalanche effect.
- Typically, hashing algorithms do not need or use a key. Remember that the purpose of hashing algorithms is to ensure the integrity of the original plaintext. Hashing algorithms have wide usage in computer systems. They are used in digital signatures and MAC algorithms, as well as for generating encryption keys, creating index data, detecting duplicate data, and creating message checksums.
- Examples of popular, modern hashing algorithms include the MD5 message-digest algorithm Whirlpool, the Secure Hash Algorithm (SHA), and its different versions, that is, SHA-1, SHA-256, and SHA-512. These three versions of SHA are supported by the Salesforce Crypto class.

The following diagram explains the general mechanism of a hashing algorithm:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_117_0.png)

Figure 3.6 – A diagram of the way hashing algorithms work

## MAC Algorithms

MAC algorithms are similar to hashing algorithms in that both generate a one-way digest/hash of the plaintext. However, MAC algorithms are mainly created to _verify the authenticity of the message sender_, in addition to verifying that the message itself has not been tampered with.

To do that, MAC algorithms use a key in the process. Typically, this is used to encrypt the hashed value itself. The key should be known to both the sender and the receiver. It can be used to verify that the entity that generated the hash owns a copy of the same key, which is proof of the authenticity of the message itself.

Examples of popular modern MAC algorithms include the **keyed-hash message authentication code (HMAC)** and its different versions, that is, HMACMD5, HMACSHA1, HMACSHA256, and HMACSHA512. All these versions are supported by the Salesforce `Crypto` class.

The following diagram explains the general mechanism of a MAC algorithm:

![](img/a3b1eecf3c87c814267f2877e8a8be1a_118_0.png)

Figure 3.7 – An illustration of the way MAC algorithms work

## Asymmetric Cryptography Algorithms

These are also known as **public-key cryptography algorithms** and are designed to create an encryption mechanism where two communicating entities can use a pair of different keys. The first is called a private key, which should not be revealed, while the second is called a public key, which is safe to be distributed.

In these algorithms, an application can encrypt a message using the public key. This message can only be decrypted using the paired private key. It is just like having a bag with a lock that can be locked with one key but unlocked using another key. The process of generating the key pair for asymmetric cryptography algorithms is more complex than the process used to generate symmetric keys. Moreover, asymmetric algorithms are typically more complex and time-consuming to run.

Asymmetric cryptography algorithms are widely adopted today. They are mainly used for the following two use cases:

- Message confidentiality: It is completely safe to distribute the public key. However, the private key must be kept secret alongside the entity that should receive and decrypt the cipher messages.

In this case, asymmetric algorithms solve one of the main challenges that are experienced with symmetric algorithms, which is distributing the key. Using an asymmetric algorithm, the message is encrypted with the public key and can only be decrypted using the private key by the entity that originally issued both key pairs.

In this mechanism, the owner of the private key can ensure the confidentiality of the received messages. Examples of well-known asymmetric cryptography algorithms include ElGamal, RSA, and the Diffie-Hellman key exchange.

- Digital signature: In this case, the private key is used to effectively sign a particular message by generating a digest/hash value of that message. The signature can be verified by anyone who has access to the public key. The verification process proves that the entity that signed the message has access to the private key and that proves the authenticity of the sender.

Moreover, hash values are used to ensure that a particular message has not been tampered with. This might sound similar to what was mentioned previously about MAC algorithms, but you need to remember that the public key is used to verify the digital signature. Distributing public keys is never a problem. Examples of some popular asymmetric cryptography algorithms that are used for digital signatures include RSA-SHA1, RSA-SHA256, RSA-SHA384, and RSA-SHA512, all of which are supported by Salesforce’s Crypto class.

Now that you have covered both types of cryptography algorithms and their variations and key use cases, let’s go through a real-life example that you come across every day while browsing secure websites.

## Cryptography Use Cases

First, the details of how TLS works will be discussed. Then, you will move on and look at a special case of it known as two-way TLS (also known as two-way SSL or mutual authentication). As a Salesforce Architect, you are likely going to come across both topics in your projects. It is very important to understand how both work as you might be asked to explain that during the review board or in real life.

## Understanding How TLS Works

TLS is a cryptography protocol that has superseded the Secure Sockets Layer (SSL). It is designed to secure any communication between two computer applications over a network (such as the internet). There have been four versions of TLS so far (1.0, 1.1, 1.2, and 1.3). Salesforce keeps updating its platform to support the latest and most secure versions of TLS. You should aim to use the latest available version of TLS whenever possible and avoid any versions earlier than 1.2.

When a communication channel is secured by TLS, the communication between two parties (normally referred to as the *client*, such as a browser, and the *server*, such as `packtpub.com`) has one or more of the following properties:

- * **Private connection**: A symmetric encryption algorithm is used to encrypt the data transmitted. The key used for this encryption is unique per session and will be generated as part of a *handshake* process. The handshake process is secure, even against a man-in-the-middle attack.
- * **Authentic identity**: The identity of the two communicating parties can be verified using an asymmetric cryptography process that utilizes *CA-signed certificates*. Verifying the identity of the communicating parties can be made optional, but it is normally required at least for the server. An example is when the client attempts to open an HTTPS website. If both parties’ identities are verified, then this becomes a two-way TLS-secured channel. You will learn how that works shortly.
- * **Reliable**: Each exchanged message will also include a MAC value to ensure that the message itself has not been modified or tampered with.

Now, look at the high-level details of what happens when a user attempts to navigate to a secure website such as `https://packt.link/LWerl`:

- 1. The client (the browser, in this case) starts the handshake process. The client provides a list of the cipher suites (and their versions) that it is willing to support.
- 2. The server (`https://packt.link/LWerl`, in this case) checks whether it is willing to support and use any of the cipher suites provided by the client. For example, Salesforce stopped supporting TLS 1.1 for its production orgs on October 25, 2019. If the browser is configured to use a version no later than TLS 1.1, then the connection will be denied. If the server agrees to use one of the cipher suites supported by the client, it will send a response containing its CA-signed certificate. (Remember, the certificate will also contain the public key.)
- 3. The client will validate the given certificate with the mentioned CA (verifying digital signature). This step is required to assure the client that the website it is communicating with is authentic and indeed what it is claiming to be. If the certificate is verified successfully, the client extracts the public key from the certificate.
- 4. The client generates a random pre-master key and encrypts it with the public key. This means that this pre-master key can only be decrypted by the holder of the private key. The client now sends this encrypted pre-master key to the server.
- 5. The server uses its private key to decrypt the pre-master key.
- 6. Both the client and the server use the pre-master key to compute a new shared key called a **shared secret** or **session key**. *Step 4, Step 5*, and *Step 6* can also be achieved using the Diffie-Hellman key exchange, but this is outside the scope of this book.
- 7. The client sends a new test message that is encrypted using the session key. This is now simple symmetric encryption.

