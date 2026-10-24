In the following section, you will be introduced to the design and mechanism of a currently dominant block cipher know as AES. First, some history will be presented with regard to Data Encryption Standard (DES) that led to the development of a new AES standard.

# Data Encryption Standard (DES)

DES was introduced by the US **National Institute of Standards and Technology (NIST)** as a standard algorithm for encryption and was in main use during 1980s and 1990s, but it has been not proven to be very resistant against brute force attacks, due to advances in technology and cryptography research. Especially in July 1998, **Electronic Frontier Foundation (EFF)** broke DES using a special purpose machine. DES uses a key of only 56 bits, which has raised some concerns. This problem was addressed with the introduction of **Triple DES (3DES)**, which proposed the usage of a 168-bit key using three 56-bit keys and the same number of executions of the DES algorithm, thus making brute force attacks almost impossible. But other limitations, such as slow performance and 64-bit block size, are not desirable.

# Advanced Encryption Standard (AES)

In 2001, after an open competition, an encryption algorithm named Rijndael that was invented by cryptographers *Joan Daemen* and *Vincent Rijmen* was standardized as AES with minor modifications by NIST in 2001. So far, no attack has been found against AES that is better than the brute force method. Original Rijndael allows different key and block sizes of 128-bit, 192-bit, and 256-bits, but in the AES standard, only a 128-bit block size is allowed. However, key sizes of 128-bit, 192-bit, and 256-bit are allowed.

# AES steps

During the AES Algorithm processing, a 4 by 4 array of bytes knows as *state* is modified using multiple rounds. Full encryption requires 10 to 14 rounds depending on the size of the key. The following table shows the key sizes and the required number of rounds:

| Key size | Number of rounds required |
|---|---|
| 128-bit | 10 rounds |
| 192-bit | 12 rounds |
| 256-bit | 14 rounds |