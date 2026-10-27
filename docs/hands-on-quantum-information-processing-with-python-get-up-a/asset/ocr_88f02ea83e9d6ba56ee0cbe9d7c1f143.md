Since Bob's public key is also known by Alice (and Eve for that matter), Alice can then compute.

(g^b)^(a) mod p =
(g^a)^(b) mod p

Therefore, Alice and Bob have eventually generated a shared key, g^ab mod p.

This is the same for both Alice and Bob. However, since Eve has no access to either a or b, she cannot compute the shared key given above. That way, Alice and Bob have managed to securely exchange a cryptographic key that can be used for the encryption of information.

A Python code for implementing the Diffie-Hellman scheme is shown as follows:

```
import math

p = input("Enter the shared prime number: \n")
p = int(p)
g = input("Enter the shared base: \n")
g = int(g)

a = input("Enter Alice's secret key: \n")
a = int(a)
b = input("Enter Bob's secret key: \n")
b = int(b)

AlicePublicKey = (g ** a) % p
BobPublicKey = (g ** b) % p

AliceSecret = (BobPublicKey ** a) % p
BobSecret = (AlicePublicKey ** b) % p

print(AliceSecret)
print(BobSecret)
```

The preceding code snippet can be summarized as follows. First, the required parameters are entered. These parameters include p, g, a, and b discussed earlier. Since, in Python, the input is, by default, a string datatype, these inputs are then converted into integer datatypes using the int() function.