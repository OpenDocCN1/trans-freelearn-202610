

## 12 个安全方面

在本章中，我们将讨论安全的不同方面。你将了解用户数据输入的验证以及我们 Web 应用程序中的安全最佳实践。本章涵盖以下主题：

- Web 安全
- 保护服务器安全
- 保护客户端安全
- 安全最佳实践

### Web 安全

犯罪是一种困扰许多人心灵的疾病。黑客对从个人邮箱凭据到银行账户详情的一切都感兴趣。维护安全的责任在于 Web 开发人员。开发人员应使用 HTTPS 来访问包含敏感数据的网页和资源。

### 传输层安全与安全套接字层概览

安全套接字层（SSL）是当今互联网上最常用的协议之一。SSL 能够保护任何基于 TCP 的传输。传输层安全（TLS）协议是 SSL 的后继者，基于较旧的 SSL 规范。TLS 1.0 版于 1999 年 1 月首次被定义为 SSL 3.0 版的升级版。两者都基于非对称密钥来加密数据，并通过数字证书在不受信任的第三方中进行身份验证。我们在客户端-服务器模型中使用 TLS，但客户端通常不提供证书。相反，服务器负责通过签名证书进行自身身份验证，并通过公钥和私钥进行加密。

用于网页浏览、电子邮件、互联网传真、即时消息和互联网语音传输（VoIP）的协议有多个版本。TLS 具有以下优点：

- 它加密信息
- 它提供身份验证
- 它支持网站上的信用卡支付
- 它防范网络钓鱼
- 它增强品牌力量并提升客户信任

在互联网上提交的信息在到达最终目的地之前会经过网络中的多个节点，因此可能被第三方获取。TLS 证书会将随机字符插入原始信息中，使其变得面目全非，只有正确的加密密钥才能帮助解密。服务器证书是另一种保护类型，当服务器所有者获得 TLS 证书后就会颁发。该证书可供客户端验证 TLS 证书是否是最新的，以及客户端的信息是否被发送到正确的位置。使用信用卡支付的在线企业必须符合支付卡行业标准。这意味着服务器需要具备至少 128 位加密强度的 TLS 证书。在线企业通常提供站点徽章和其他品牌标识，以表明正在使用受信任的加密。这些信息给客户带来额外的保障，并在客户与企业之间建立信任。

### TLS 证书

要在 TCP/IP 协议栈中确定在哪个层级处理 TLS 协议确实非常复杂。TLS 安全协议描述了应如何使用算法以及 TLS 证书如何建立安全连接。要获得证书，你必须在服务器上创建证书签名请求（CSR）。此过程会在服务器上生成一对私钥和公钥。然后，你必须将包含公钥的 CSR 数据文件发送给证书颁发机构（CA），以获取 TLS 证书。CA 会根据 CSR 文件创建一个数据结构，以便将来与私钥匹配。CA 永远不会看到私钥，因此它不会被泄露。一旦收到 TLS 证书，你就可以将其安装到服务器上。Dart 使用 Mozilla 的网络安全服务（NSS）库来处理 TLS。我们需要使用 NSS 安全工具中的证书数据库工具 `certutil` 来操作证书数据库。你可以获取源代码并为你的平台快速构建 `certutil`，但我在我的 Ubuntu 工作站上安装了以下预构建版本的程序：

```
sudo apt-get install libnss3-tools
```

`-S` 命令选项会创建单个证书并将其添加到证书数据库中。`-s` 选项后面的文本提供了标识证书所有者的主题。`-x` 选项告诉 `certutil` 命令所创建的证书是自签名的。`-v` 选项设置新证书的有效月数。`-m` 选项为正在创建的证书设置唯一的序列号。当我们运行 `certutil` 命令时，它会要求我们在键盘上按键以生成随机种子，该种子将用于生成我们的密钥。

3. 我们现在有了 CA 证书，并且需要生成密钥和证书签名请求。让我们使用以下命令来完成：

```
certutil -R -s "CN=localhost, O=Mastering Dart, L=Cape Town, ST=WC, C=CA" -p "+27 21 1234567" -o mycert.req -d sql:/home/akserg/Project/server/bin/pkcert
```

`-R` 命令选项会创建一个证书请求文件，该文件可以提交给 CA 以处理成最终证书。我们指定主题来标识证书所有者；在本例中，所有者是我。其他信息（如你的电话号码）也可以作为输入。输出默认为标有 `-o` 选项的输出文件。当我们运行 `certutil` 命令时，它会要求输入密码，我们可以再次使用随机种子生成密钥。

4. 现在，我们可以使用以下命令查看数据库中的密钥列表：

```
certutil -K -d sql:/home/akserg/Project/server/bin/pkcert
```

结果如下：

```
< 0> rsa e22c881d9eb382ea69257410cf464dfedcd49354   NSS Certificate DB:CACert
< 1> rsa b909266e0d5a14523158bfc7903ea9460fad2da6   (orphan)
```

5. 我们需要使用以下命令对密钥进行签名：

```
certutil -C -m 2345 -i mycert.req -o mycert.crt -c CACert -d sql:/home/akserg/Project/server/bin/pkcert
```

6. 最后，是时候使用以下命令将证书添加到数据库中了：

```
certutil -A -n localhost_cert -t "p,p,p" -i mycert.crt -d sql:/home/akserg/Project/server/bin/pkcert
```

在 `-n` 选项之后，我们的证书名称是 `localhost_cert`。

7. 你可以使用以下命令查看特定证书的信息：

```
certutil -L -n localhost_cert -d sql:/home/akserg/Project/server/bin/pkcert
```

结果如下：

```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 2345 (0x929)
        Signature Algorithm: PKCS #1 SHA-1 With RSA Encryption
        Issuer: "CN=CA Issuer"
        Validity:
            Not Before: Thu Aug 21 17:15:05 2014
            Not After : Fri Nov 21 17:15:05 2014
        Subject: "CN=Sergey Akopkokhyants,O=Mastering Dart,L=Cape Town,ST=WC, C=CA"
        Subject Public Key Info:
            Public Key Algorithm: PKCS #1 RSA Encryption
...
```

8. 或者，你可以使用以下命令验证特定证书：

```
certutil -V -n localhost_cert -b 9803201212Z -u SR -e -1 -d sql:/home/akserg/Project/server/bin/pkcert
```

结果如下：

```
localhost_cert : Peer’s Certificate has expired.
localhost_cert : Peer’s certificate has been marked as not trusted by the user.
```

既然我们已经完成了自签名证书的创建，是时候回到服务器代码并对其进行安全保护了。

### 保护服务器安全

打开 `server.dart` 文件并输入以下代码：

```
import 'dart:io';

main() {
  var pkcertDB = Platform.script.resolve('pkcert').toFilePath();
  SecureSocket.initialize(database: pkcertDB,
    password: 'changeit');

  HttpServer
    .bindSecure(InternetAddress.ANY_IP_V6, 8443,
      certificateName: 'localhost_cert')
    .then((server) {
      server.listen((HttpRequest request) {
        request.response.write('Hello, world!');
        request.response.close();
      });
    });
}
```

这是著名的“Hello, World!”示例的实现。我仅在演示目的下把证书密码写在代码中。请将你的密码保存在外部加密文件中。服务器代码非常简单。唯一的小区别是引用了 `SecureSocket` 而不是 `Socket` 类。通过调用该类的静态 `initialize` 方法，我们初始化了 NSS 库。现在，我们应该通过 `HttpServer` 的静态 `bindSecure` 方法来组织绑定，以创建 HTTPS 服务器。让我们运行它并在 Dartium 浏览器中打开以下 URL：

```
https://localhost:8443/index.html
```

所有魔法，如 TLS 握手、密钥和消息交换，都在幕后发生。由于我们服务器的证书是自签名的，Web 浏览器会告知我们这一事实，如以下屏幕截图所示：

![](img/52c05b53a56abd53584e4fed738ed509_528_0.png)

