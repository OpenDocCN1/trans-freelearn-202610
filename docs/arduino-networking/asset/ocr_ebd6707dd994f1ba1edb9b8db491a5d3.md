# # Creating a web server on Arduino

For the rest of this chapter, we are going to take another approach. We are still going to make the Arduino with the Ethernet Shield work as an independent system, but this time we are even going to create a web server on the Arduino board. This is actually similar to the project we saw in the Chapter 2, Sending Data to a Web Server, but this time it is the Arduino board itself that will display the data as it is measured.

The sketch for this part starts by including the right libraries, as shown in the following code:

```
```cpp
#include "DHT.h"
#include <Ethernet.h>
#include <SPI.h>
```
```

Define the pin and the type of the sensor in the following manner:

```
```cpp
#define DHTPIN 7
#define DHTTYPE DHT11
```
```

Also, you have to define the MAC address of your Ethernet shield as usual, as shown in the following line of code:

```
```cpp
byte mac[] = { 0x90, 0xA2, 0xDA, 0x0E, 0xFE, 0x40 };
```
```

We will also define a default IP address in case the DHCP fails, as shown in the following line:

```
```cpp
IPAddress ip(192,168,1,50);
```
```

This is where the new feature comes in. So far, we only used the EthernetClient class to define a client that will connect to a remote server. Here, however, because we want to create a server right on the Arduino board, we are going to use the EthernetServer class, as shown in the following line of code:

```
```cpp
EthernetServer server(80);
```
```

You can use the port 80 for your server, as it will be much easier to access the server from your web browser.

We also should not forget to create an instance of the DHT sensor. This is done using the following line:

```
```cpp
DHT dht(DHTPIN, DHTTYPE);
```
```

Now, in the setup() function of the sketch, we will try to use DHCP to get an IP address for the Ethernet shield using the following code:

```
```cpp
if (Ethernet.begin(mac) == 0) {
    Serial.println("Failed to configure Ethernet using DHCP");
    Ethernet.begin(mac, ip);
}
```
```

Still in the setup() function, we also have to start our Ethernet server, and print out the IP