As the output in Figure 16.5 shows, PoshC2 generates a variety of payloads and stores them in the /var/poshc2/Book_ENT-C2/payloads/ folder. We are going to copy the payloads into the /var/www/html/files/payloads folder of the Kali Linux machine so that we can expose them via the apache2 web server. The following output shows the copying of the payload files and the starting of the apache service:

```
$ sudo mkdir /var/www/html/files/
$ sudo cp /var/poshc2/Book_ENT-C2/payloads/ /var/www/html/files -r
$ sudo service apache2 start
```

And with that, we are ready to grab a fitting PoshC2 implant payload from the Terminal server at Company Z.

From our RDP session into the test server, we open Internet Explorer and navigate to http://222.222.222.222/files, as illustrated in the following screenshot:

![](bbox=[0.295, 0.335, 0.685, 0.715])

Figure 16.6 – List of payloads for PoshC2 on the Kali web server