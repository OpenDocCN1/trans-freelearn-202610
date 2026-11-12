answer on the Serial Monitor for debugging purposes, using the following piece of code:

```
while (client.connected()) {
while (client.available()) {
char c = client.read();
Serial.print(c);
}
}
```

Finally, when it is over, we stop the connection. This is done with the following code:

```
if (!client.connected()) {
Serial.println();
Serial.println("disconnecting.");
client.stop();
}
```

We repeat the entire process every 10 seconds with the following line:

```
delay(10000);
```

# Note

All the code for this chapter can be found on the GitHub repository of this chapter at https://github.com/openhomeautomation/arduino-networking/tree/master/chapter5.

It is now time to test the sketch and upload data to Xively. You can upload the sketch to Xively at this point, and open the Serial Monitor in the Arduino IDE. You should see that the IP address of the board is being printed out, and that the sketch is connected to the Xively server:

**IP address: 192.168.1.104
Connecting...**

You should also see how the data was formatted for Xively:

```
{ "version": "1.0.0", "datastreams" : [ { "id" : "Temperature", "current_value" : "24"},{"id" : "Humidity", "current_value" : "37"}]}
```

After a moment, you should see the answer coming back from Xively. If the data was sent correctly to the Xively server, you should see a 200 OK code being printed out to the Serial Monitor:

```
HTTP/1.1 200 OK
Date: Wed, 21 May 2014 15:02:49 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 0
Connection: close
X-Request-Id: e44becaa5231354568262013fa713d9f099ffc83
Cache-Control: max-age=0
Vary: Accept-Encoding
```

This means that the data was correctly received by the Xively server, and in an understandable format.

If that is not the case, there are several things you can check. First, make sure that the