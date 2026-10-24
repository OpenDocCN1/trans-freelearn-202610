# Base58Check encoding

This encoding is used to limit the confusion between various characters, such as 0OIl as they can look the same in different fonts. The encoding basically takes the binary byte arrays and converts them into human-readable strings. This string is composed by utilizing a set of 58 alphanumeric symbols. More explanation and logic can be found in the base58.h source file in the bitcoin source code.

```
6 /**
7 * Why base-58 instead of standard base-64 encoding?
8 * - Don't want 0OIl characters that look the same in some fonts and
9 *     could be used to create visually identical looking data.
10 * - A string with non-alphanumeric characters is not as easily accepted as input.
11 * - E-mail usually won't line-break if there's no punctuation to break at.
12 * - Double-clicking selects the whole string as one word if it's all alphanumeric.
13 */
14 #ifndef BITCOIN_BASE58_H
```

Explanation from the bitcoin source code

Bitcoin addresses are encoded using the Base58check encoding.

# Vanity addresses

As bitcoin addresses are based on base 58 encoding, it is possible to generate addresses that contain human-readable messages. An example is shown as follows:

![](bbox=[0.38, 0.65, 0.63, 0.88])

Public address encoded in QR