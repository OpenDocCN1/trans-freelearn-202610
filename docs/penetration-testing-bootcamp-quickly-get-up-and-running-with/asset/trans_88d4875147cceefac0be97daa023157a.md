
## 利用 whois 进行信息收集

当域名注册时，必须输入一些特定的联系信息。你可以选择公开显示这些信息，或支付费用将其隐藏，以免他人看到这些个人信息。其中包括姓名、物理地址、电子邮件地址和电话号码等项目。除了联系信息外，还有其他相关信息，例如域名注册日期和分配的域名服务器。这些信息可能很有用。

对公司的域名使用 whois 可以让我们看到一些相关信息，例如谁维护 DNS 记录及其联系信息。利用注册日期可以知道域名可能何时到期，如果公司忘记续费，你也可以利用这些信息在到期时自行注册。过去这种情况曾发生在许多知名网站上。

以下是关于 google.com 域名信息的 whois 截图：

```
root@kali:~# whois -H google.com

Whois Server Version 2.0

Domain names in the .com and .net domains can now be registered
with many different competing registrars. Go to http://www.internic.net
for detailed information.

Domain Name: GOOGLE.COM
Registrar: MARKMONITOR INC.
Sponsoring Registrar IANA ID: 292
Whois Server: whois.markmonitor.com
Referral URL: http://www.markmonitor.com
Name Server: NS1.GOOGLE.COM
Name Server: NS2.GOOGLE.COM
Name Server: NS3.GOOGLE.COM
Name Server: NS4.GOOGLE.COM
Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited
Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited
Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited
Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited
Updated Date: 20-jul-2011
Creation Date: 15-sep-1997
Expiration Date: 14-sep-2020

>>> Last update of whois database: Sun, 15 Jan 2017 03:43:20 GMT <<<
```

以下是从 whois 屏幕中可以看到的联系信息：

```
Registry Registrant ID:
Registrant Name: Dns Admin
Registrant Organization: Google Inc.
Registrant Street: Please contact contact-admin@google.com, 1600 Amphitheatre Parkway
Registrant City: Mountain View
Registrant State/Province: CA
Registrant Postal Code: 94043
Registrant Country: US
Registrant Phone: +1.6502530000
Registrant Phone Ext:
Registrant Fax: +1.6506188571
Registrant Fax Ext:
Registrant Email: dns-admin@google.com
Registry Admin ID:
```

现在，你可以看到 whois 提供了一些很棒的信息。如果没有域名只有 IP 地址怎么办？没问题！whois 可以使用 IP 地址进行查询。事实上，这是我最喜欢使用 whois 的方式，因为它能提供有关公司的一些关键信息。例如，它会显示该 IP 所属的分配网络块以及公司的 AS 编号。接下来，我将向你展示如何使用这个 AS 编号。

以下是使用 IP 地址进行 whois 查询的截图，可以看到它与查询域名时的结果大不相同。我运行了以下命令来获取输出：

```
whois -H 216.58.218.238
```

![](img/095df2cd1930baecbcecaa51ee3417aa_36_0.png)

让我们回到之前找到的 AS 编号。我们现在可以取这个编号并查找分配给该公司（包括 IPv4 和 IPv6）的所有 IP 网络。这也可以通过 whois 和一些参数来实现，同时使用 Unix 管道和 grep 过滤输出。利用此工具是验证利益相关方所声称拥有的 IP 地址范围并获取额外攻击面的绝佳方式。我见过很多利益相关方忘记或不知道自己拥有的网络块的情况：

```
root@kali:~# whois -h whois.radb.net -- '-i origin AS36856' | grep route
route:       63.245.208.0/20
route:       63.245.223.0/24
route:       63.245.208.0/22
route:       63.245.221.0/24
route:       63.245.220.0/24
route:       63.245.214.0/23
route:       63.245.219.0/24
route:       63.245.208.0/20
route:       63.245.221.0/24
route:       63.245.214.0/23
route6:      2620:101:8001::/48
route6:      2620:101:8000::/40
route6:      2620:101:8000::/40
```

## 使用 dnsmap 枚举 DNS

dnsmap 是一个非常棒的工具，可以在你正在查找的域名内发现子域名。这是可以查看是否存在其他可能已知或未知的公开站点（内部和/或外部）的绝佳方式。这使你有可能找到并利用未被正确控制或管理的子域名。你可以提供自己的词表来对照子域名进行查找，也可以直接使用内置的词表。有些组织会将部分子域名外包给第三方，因此在使用这些信息时需要谨慎。工作范围可能仅涵盖你受雇的公司；因此，如果你试图主动利用它，另一家组织可能不会对你持友好态度。这应该在利益相关方会议上确定，但有时事情还是会遗漏。以下是 dnsmap 命令行选项的截图：

```
root@kali:~# dnsmap
dnsmap 0.30 - DNS Network Mapper by pagvac (gnucitizen.org)

usage: dnsmap <target-domain> [options]
options:
-w <wordlist-file>
-r <regular-results-file>
-c <csv-results-file>
-d <delay-millisecs>
-i <ips-to-ignore> (useful if you're obtaining false positives)

e.g.:
dnsmap target-domain.foo
dnsmap target-domain.foo -w yourwordlist.txt -r /tmp/domainbf_results.txt
dnsmap target-domain.foo -r /tmp/ -d 3000
dnsmap target-domain.foo -r ./domainbf_results.txt
```

然后我们可以取正在查看的域名，在本例中为 https://www.mozilla.org，并查看可能存在哪些其他子域名。以下截图显示了针对 Mozilla.org 运行 dnsmap 命令的输出：

![](img/095df2cd1930baecbcecaa51ee3417aa_38_0.png)

有了这些信息，我们现在可以开始探测过去可能不知道的额外 IP 地址/站点，这可以增加我们可操作的攻击/渗透测试面。目标越多，我们进入的机会就越大，并且能够将其包含在我们将交付的渗透测试报告中。

## 使用 DNSRecon 进行 DNS 侦察

DNSRecon 是一个方便的基于 DNS 的实用工具，可让你对 DNS 记录执行各种侦察操作。首先，你可以枚举所有不同类型的 DNS 记录，检查任何缓存条目，并检查区域传输，甚至可以枚举 Google 以获取额外的区域文件。

DNSRecon 并不是一个复杂的工具，但它功能强大，可以节省你手动使用其他工具花费的大量时间。`dnsrecon` 包有一些命令行选项，因此一定要查看主页面，看看在你的环境中哪些是相关的。我通常使用 `-g` 开关进行 Google 枚举。这可以找到引用我正在查找的主域名的额外子域名。这让我不必手动在 Google 上查找，从而节省了大量时间。这是可以自动化部分 `google-fu` 的绝佳方式，可以获得一些很棒的信息。

以下是实验室中使用 `-g` 开关的示例。你会注意到在命令的第一部分会给出标准信息。Google 枚举结果稍后出现。以下是通过 DNS 查找发现的一般枚举结果：

```
root@kali:~# dnsrecon -d cnn.com -g
[*] Performing General Enumeration of Domain: cnn.com
[-] DNSSEC is not configured for cnn.com
[*] SOA ns-47.awsdns-05.com 205.251.192.47
[*] NS ns-1086.awsdns-07.org 205.251.196.62
[*] NS ns-1086.awsdns-07.org 2600:9000:5304:3e00::1
[*] NS ns-1630.awsdns-11.co.uk 205.251.198.94
[*] NS ns-1630.awsdns-11.co.uk 2600:9000:5306:5e00::1
[*] NS ns-47.awsdns-05.com 205.251.192.47
[*] MX ppsprmsa.turner.com 157.166.168.210
[*] MX ppsprmsh.turner.com 157.166.157.29
[*] A cnn.com 151.101.64.73
[*] A cnn.com 151.101.128.73
[*] A cnn.com 151.101.192.73
[*] A cnn.com 151.101.0.73
[*] TXT cnn.com 598362927-4422061
[*] TXT cnn.com 321159687-4422031
[*] TXT cnn.com v=spf1 include:cnn.com._nspf.vali.email include:%{i}._ip.%{h}._ehlo.%{d}._spf.vali.email ~all
[*] TXT cnn.com globalsign-domain-verification=21I5pahhCu_jg_2RC5GEdo1QmAa4K7rhP7_OA-1Z8K
[*] TXT cnn.com 353665828-4422052
[*] Enumerating SRV Records
[*] SRV _sip._tls.cnn.com sipdir.online.lync.com 146.112.61.106 443 1
[*] SRV _sip._tls.cnn.com sipdir.online.lync.com ::ffff:146.112.61.106 443 1
[*] SRV _sipfederationtls._tcp.cnn.com sipfed.online.lync.com
```
