The Telnet terminals will show output as follows:

```
pi@raspberrypi ~/book/chapter14 $ telnet localhost 8888
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
Welcome to the server. Type something and hit enter
hi
OK...hi
asd
OK...asd
cv
OK...cv
```

The server terminal will look like this:

```
pi@raspberrypi ~/book/chapter14 $ python prog10.py
Socket created
Socket bind complete
Socket now listening
Connected with 127.0.0.1:60730
Connected with 127.0.0.1:60731
```

This connection handler takes the input from the client and replies with the same.

# Looking back

By now, we have learned the basics of socket programming in Python. When testing these programs, if you face the following error, simply change the port number, and the server will run fine:

```
Bind failed. Error Code : 98 Message Address already in use
```

# A Telnet client in Python

The Telnet client is a simple command-line program that is used to connect to socket servers and exchange messages. The following is an example of how to use Telnet to connect to https://www.google.com and fetch the homepage:

```
$ telnet www.google.com 80
```