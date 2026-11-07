As you can see in the above screenshots, I have successfully placed my inside host within that conversation. I can also dump that capture to a file and verify the connection in Wireshark by following the TCP stream:

```
HTTP/1.1 304 Not Modified
Content-Type: image/png
Last-Modified: Tue, 15 Nov 2016 16:47:40 GMT
Cache-Control: max-age=78870
Expires: Thu, 09 Feb 2017 23:57:51 GMT
Date: Thu, 09 Feb 2017 02:03:21 GMT
Connection: keep-alive

GET /newimages/large/nfew.png HTTP/1.1
Host: forecast.weather.gov
Connection: keep-alive
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36
Accept: image/webp,image/*,*/*;q=0.8
Referer: http://www.forecast.weather.gov/MapClick.php?
lat=40.60688295038892&lon=-75.50721787340535
Accept-Encoding: gzip, deflate, sdch
Accept-Language: en-US,en;q=0.8
Cookie: __utma=132404377.747112102.1480382093.1486552473.1486587909.8;
__utmc=132404377; __utmz=132404377.1486587909.8.utmcsr=weather.gov|utmccn=(referral)|utmcmd=referral|utmcct=/ctp/; QSI_HistorySite=http%3A%2F%2Fforecast.weather.gov%3FMapClick.php%3F%26x%3D76%26y%3D100%26site%3Dphi%26zmx%3D%26zmy%3D%26map_x%3D76%26map_y%3D100%23.Wjr9mBjrJZo-14865524731.46%7Chttp%3A%2F%2Fforecast.weather.gov%2FMapClick.php%3F%26x%3D76%26y%3D100%26site%3Dphi%26zmx%3D%26zmy%3D%26map_x%3D76%26map_y
```

As you can see, it is quite easy to get into the middle of a conversation. But what happens if you prefer a GUI-based MiTM tool? Well, let us take a look at Ettercap.

# Ettercap

Ethercap is a great tool for performing an MiTM attack using the GUI as opposed to the CLI. Ettercap has a CLI-based utility, but the GUI version is quite powerful and a great alternative to arpspoof.

When starting up Ethercap, you first need to decide which mode you want to run it in. The available options are either Bridge sniffing or Unified sniffing. If you are physically wired inline, you can select **Bridge sniffing**. But if you are not, you should select **Unified sniffing**, which is what I will select: