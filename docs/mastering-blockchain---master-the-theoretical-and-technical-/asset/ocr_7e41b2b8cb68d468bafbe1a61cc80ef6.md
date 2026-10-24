Once the state is initialized with the input to the cipher, four operations are performed in four stages in order to encrypt the input. These stages are AddRoundKey, SubBytes, ShiftRows, and MixColumns:

+   1. In the AddRoundKey step, the state array is XORed with a subkey, which is derived from the master key.
+   2. This is the substitution step where a lookup table (S-box) is used to replace all bytes of the state array.
+   3. This step is used to shift each row except the first one in the state array to the left in a cyclic and incremental manner.
+   4. Finally, all bytes are mixed in this step in a linear fashion column-wise.

The preceding steps describe one round of AES. In the final round (either 10, 12, or 14 depending on the key size), stage 4 is replaced with Addroundkey to ensure that the first three steps cannot be simply inverted back:

![](bbox=[0.348, 0.416, 0.652, 0.661])

Various cryptocurrency wallets use AES encryption to encrypt locally stored data. Especially in bitcoin wallet, AES 256 in the CBC mode is used.

# An OpenSSL example of how to encrypt and decrypt using AES

```
```text
:~/Crypt$ openssl enc -aes-256-cbc -in message.txt -out message.bin
enter aes-256-cbc encryption password:
Verifying - enter aes-256-cbc encryption password:
:~/Crypt$ ls -ltr
total 12
```
```