
在示例中，左侧的图显示了满足此处所示方程的点：

$$y^2 = x^3 + 7x + 11$$

在有限域 $F_{23}$ 上，前面所示的方程有 27 个解。选择将 $P$ 和 $Q$ 相加，得到点 $R$。右侧展示了计算过程，用于计算第三个点 $R$。请注意，在这里，$l$ 用于表示经过 $P$ 和 $Q$ 的直线。

为了举例说明图中的点如何满足该方程，选取一个点 $(x, y)$，其中 $x = 3$，$y = 6$。

将这些值代入方程，可以看出方程确实得到满足。如下所示：

$$y^2 \mod 23 = x^3 + 7x + 11 \mod 23$$

$$6^2 \mod 23 = 3^3 + 7(3) + 11 \mod 23$$

$$36 \mod 23 = 59 \mod 23$$

$$13 = 13$$

下一节将介绍点的倍乘的概念，这是可以在椭圆曲线上执行的另一种运算。

## 点的倍乘

![](img/62ab880d2302f959d34db648cb0bec5e_101_0.png)

表示实数域上点的倍乘的图

对于点的倍乘，方程变为如下形式：

$$x_3 = s^2 - 2x_1 \mod p$$

$$y_3 = s(x_1 - x_3) - y_1 \mod p$$

$$s = \frac{y_2 - y_1}{x_2 - x_1} \mod p$$

这里，s 是经过 P 的切线的斜率。它是上图中顶部的直线。在前面的示例中，为了简单起见，曲线是在实数域上绘制的，没有展示方程的解。

这里展示了一个示例，它显示了有限域 $F_{23}$ 上椭圆曲线的解和点的倍乘。左侧的图显示了满足以下方程的点：

$$y^2 = x^3 + 7x + 11$$

![](img/62ab880d2302f959d34db648cb0bec5e_102_0.png)

使用 Certicom 的在线计算器工具进行点的倍乘的示例

如上所示，右侧展示了将 $P$ 与自身相加（点的倍乘）后求出 $R$ 的计算过程。这里没有 Q，因为使用同一个点 $P$ 进行倍乘。请注意，在计算中，$l$ 用于表示经过 $P$ 的切线。

下一节将介绍离散对数问题。

## 离散对数问题

ECC 中的离散对数问题基于这样一个思想：在特定条件下，椭圆曲线上的所有点构成一个循环群。

在椭圆曲线上，公钥是生成元的随机倍数，而私钥是用于生成该倍数的随机选择的整数。换句话说，私钥是一个随机选择的整数，而公钥是曲线上的一个点。

离散对数问题用于找到私钥（一个整数），该整数位于椭圆曲线上所有点的范围之内。后面的一个方程将精确地说明这一点。

考虑一个椭圆曲线 E，其中有两个元素 P 和 T。离散对数问题是要找到整数 d，其中 $1 \le d \le \#E$，使得：

$$P + P + \dots + P = dP = T$$

这里，T 是公钥（曲线上的点），d 是私钥。换句话说，公钥是生成元的随机倍数，而私钥是用于生成该倍数的整数。#E 表示椭圆曲线的阶，基本上是指椭圆曲线循环群中存在的点的数量。循环群由椭圆曲线上的点与无穷远点的组合构成。

密钥对与椭圆曲线的特定域参数相关联。域参数包括域大小、域表示、域中的两个元素 a 和 b、两个域元素 $X_g$ 和 $Y_g$、点 G 的阶 n（计算方式为 $G=(X_g, Y_g)$）以及余因子 $h = \#E(F_q)/n$。本节稍后将介绍一个使用 OpenSSL 的实际示例。

有多种被推荐并标准化用于 ECC 的曲线参数。这里向你展示一个 SECP256K1 规范的示例。这是比特币中使用的规范：

与 Koblitz 曲线 secp256k1 相关的 $F_p$ 上的椭圆曲线域参数由六元组 T = (p, a, b, G, n, h) 指定，其中有限域 $F_p$ 定义为：

```
p = FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFE FFFFFC2F
  = 2^256 − 2^32 − 2^9 − 2^8 − 2^7 − 2^6 − 2^4 − 1
```

$F_p$ 上的曲线 $E: y^2 = x^3 + ax + b$ 定义为：

```
a = 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000000
b = 00000000 00000000 00000000 00000000 00000000 00000000 00000000 00000007
```

压缩形式的基点 G 为：

```
G = 02 79BE667E F9DCBBAC 55A06295 CE870B07 029BFCDB 2DCE28D9 59F2815B 16F81798
```

非压缩形式为：

```
G = 04 79BE667E F9DCBBAC 55A06295 CE870B07 029BFCDB 2DCE28D9 59F2815B 16F81798 483ADA77 26A3C465 5DA4FBFC 0E1108A8 FD17B448 A6855419 9C47D08F FB10D4B8
```

最后，G 的阶 n 和余因子为：

```
n = FFFFFFFF FFFFFFFF FFFFFFFF FFFFFFFE BAAEDCE6 AF48A03B BFD25E8C D0364141
h = 01
```

SECP256K1 规范取自 http://www.secg.org/sec2-v2.pdf

这里将解释六元组中所有这些值的含义。

P 是用于指定有限域大小的素数 p。

a 和 b 是椭圆曲线方程的系数。

G 是生成所需子群的基点，也称为生成元。基点可以用压缩或非压缩形式表示。在实际实现中，不需要存储曲线上的所有点。压缩生成元之所以有效，是因为曲线上的点可以仅通过 x 坐标和 y 坐标的最低有效位来识别。

n 是子群的阶。

h 是子群的余因子。

在下一节中，将展示一个使用 OpenSSL 的示例，以帮助你理解 RSA 的实际应用。

在下一节中，将展示如何使用 OpenSSL 生成 RSA 公钥和私钥对。

## 如何生成公钥和私钥对

首先，展示如何使用 OpenSSL 生成 RSA 私钥。

### 私钥

```
~/Crypt$ openssl genpkey -algorithm RSA -out privatekey.pem -pkeyopt rsa_keygen_bits:1024
........................................++++++
........................................++++++
```

执行该命令后，会生成一个名为 privatekey.pem 的文件，其中包含生成的私钥。如下所示：

```
~/Crypt$ cat privatekey.pem
-----BEGIN PRIVATE KEY-----
MIICfgIBADANBgkqhkiG9w0BAQEFAASCAwggJcAgEAAoGBAKJOFBzPy2vOd6em
Bk/UGrzDy7TvgDYnYxBfiEJId/r+ByMt/F14k2fDTOVwxXaXTxiQgD+BKuiey/69
9itnrqW/xy/pocDMvobj8QCngEntOdNoVSaN+t0f9nRM3iVM94mz3/C/v4vXvoac
PyPkr/0jhIVOwoCurXGTghgqIbHRAgMBAAECgYEAlB3s/N41Jh011TkOSYunWtzT
6isnNkR7g1wRY9h+rGxxx4kP5b1DyE5vxBL6xgBle8JVQMzm5sKJrUPFZzzT5
NNNWugCxairxcF1mPzJAP3aqc5SjKpTv4qgqYevwgW1A0R3xKQz8ZKU+bT0Z2hXV
D1oHxu75mDY3xCwqSAECQODUYV04wNSEjEy9tYJ0zaryDACvd/VG2/U/6qiQGajB
eSpSqeEESigbusKku+wVtRYgWWEomL/X58t+K01eMZZAKeAw6PUR9YLebsm/Sji
IOShV4AKuFdi7t7DYWE5Ulbluqp/i28zN/ytt4BXXIs/KcFykQGeAC6LDHZyyc
ntDIOQJAVqrE1/wYvV5jkqcXbYLGv5YA+KYDOb9Y/ZRM5UETVKCVXNanF5CjfW1h
MMhfNxyGvwy2YVK0Nu8oY3xYPi+5QQJAUGcmOre4w6Cs12JUJ5p+zG0s+rG/URhw
B7djTXm7p6b6wR1EWYAZDM9MArenj8uXAA1AGCcIsmiDqHfU71gzOQJAe9mOdNGW
7qRppgmOE5nuEbxkDSQI7OqHYbOLuwfCjHzJBrSgqyi6pj9/9CbXJrZPgNDwdLEb
GgpDKtZs9gLv3A==
-----END PRIVATE KEY-----
```

### 生成公钥

由于私钥与公钥在数学上是关联的，因此可以从私钥生成或推导出公钥。以上面的私钥为例，可以按如下方式生成公钥：

```
:~/Crypt$ openssl rsa -pubout -in privatekey.pem -out publickey.pem
writing RSA key
```

可以使用文件阅读器或任何文本查看器查看公钥，如下所示：

```
:~/Crypt$ cat publickey.pem
-----BEGIN PUBLIC KEY-----
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCiThQcz8trznenpgZP1Bq8w8u0
74A2J2MQX4hCSHf6/hMjFxdeJNnw0zlcMV2108YkIA/gSronsv+vfYrZ66lv8cv
6aHAzL6G4/EAp4BJ7TnTaFUmfjrdH/Z0TN41TPeJs9/wv7+L176GnD8j5K/9I4SF
dMKArq1xk4IYKiGxOQIDAQAB
-----END PUBLIC KEY-----
```

为了查看各个组件的更多详细信息，例如模数、过程中使用的素数、指数以及所生成私钥的系数，可以使用以下命令（由于输出过大，此处未显示完整输出）：

```
:~/Crypt$ openssl rsa -text -in privatekey.pem
Private-Key: (1024 bit)
modulus:
    00:a2:4e:14:1c:cf:cb:6b:ce:77:a7:a6:06:4f:d4:
    1a:bc:c3:cb:b4:ef:80:36:27:63:10:5f:88:42:48:
    77:fa:fe:13:23:2d:fc:5d:78:93:67:c3:4c:e5:70:
    c5:76:97:4f:18:90:80:3f:81:2a:e8:9e:cb:fe:bd:
    f6:2b:67:ae:a5:bf:c7:2f:e9:a1:c0:cc:be:86:e3:
    f1:00:a7:80:49:ed:39:d3:68:55:26:8d:fa:dd:1f:
    f6:74:4c:de:25:4c:7f:89:b3:df:f0:bf:bf:8b:d7:
    be:86:9c:3f:23:e4:af:fd:23:84:85:74:c2:80:ae:
    ad:71:93:82:18:2a:21:b1:d1
publicExponent: 65537 (0x10001)
privateExponent:
    00:94:1d:ec:fc:de:25:26:1d:25:d5:39:0e:49:8b:
    a7:5a:dc:d3:ea:2b:27:36:44:7b:83:55:ab:63:d1:
    fe:ac:6f:71:c7:89:0f:e5:bd:43:c8:4d:d2:bf:10:
    4b:24:0e:b1:80:19:5e:f0:95:50:33:39:b7:b0:a2:
    6b:24:f1:59:cf:34:f9:34:d3:67:ba:00:b1:6a:2a:
    f1:70:5d:66:3f:32:40:3f:76:aa:a5:c4:a3:c4:aa:
    53:bf:8a:a0:a9:87:af:c2:05:b5:03:44:77:c4:a4:
    19:cc:12:94:f9:b4:ce:da:15:d5:0f:5a:07:c6:ee:
    f9:98:36:37:c4:2c:2a:48:01
```

