```
146.112.61.106 5061 1
[*] SRV _sipfederationtls._tcp.cnn.com sipfed.online.lync.com
::ffff:146.112.61.106 5061 1
[*] 4 Records Found
```

Here are examples of other sites found via Google enumeration:

```
[*] Performing Google Search Enumeration
[*] CNAME www.cnn.com turner.map.fastly.net
[*] A turner.map.fastly.net 151.101.192.73
[*] A turner.map.fastly.net 151.101.128.73
[*] A turner.map.fastly.net 151.101.64.73
[*] A turner.map.fastly.net 151.101.0.73
[*] CNAME edition.cnn.com www.edition.cnn.com
[*] CNAME www.edition.cnn.com turner.map.fastly.net
[*] A turner.map.fastly.net 151.101.192.73
[*] A turner.map.fastly.net 151.101.128.73
[*] A turner.map.fastly.net 151.101.64.73
[*] A turner.map.fastly.net 151.101.0.73
[*] CNAME cnnradio.cnn.com cnnradio.wordpress.com
[*] CNAME cnnradio.wordpress.com vip-lb.wordpress.com
[*] A vip-lb.wordpress.com 192.0.79.33
[*] A vip-lb.wordpress.com 192.0.79.32
[*] CNAME news.blogs.cnn.com cnnnews.wordpress.com
[*] CNAME cnnnews.wordpress.com vip-lb.wordpress.com
[*] A vip-lb.wordpress.com 192.0.79.33
[*] A vip-lb.wordpress.com 192.0.79.32
[*] CNAME religion.blogs.cnn.com cnnreligion.wordpress.com
[*] CNAME cnnreligion.wordpress.com vip-lb.wordpress.com
[*] A vip-lb.wordpress.com 192.0.79.33
[*] A vip-lb.wordpress.com 192.0.79.32
[*] CNAME ac360.blogs.cnn.com cnnac360.wordpress.com
[*] CNAME cnnac360.wordpress.com vip-lb.wordpress.com
```

There are some important outputs in this screen. First, you can see that there is no DNSSEC configure. This is an important find that can be noted in the report. You can also notice the SPF records that let us know which servers can send email. Also notice that they are not using IPv6. I can also see they are using Lync services due to some of the records that exist, so definitely some Microsoft services are in their environment.