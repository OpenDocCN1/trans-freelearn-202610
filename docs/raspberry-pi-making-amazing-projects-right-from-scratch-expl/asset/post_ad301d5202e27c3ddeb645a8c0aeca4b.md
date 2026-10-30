
The server terminal will show the following:

```
pi@raspberrypi ~/book/chapter14 $ python prog9.py
Socket created
Socket bind complete
Socket now listening
Connected with 127.0.0.1:60225
Connected with 127.0.0.1:60237
Connected with 127.0.0.1:60239
```

So now, the server is up and the Telnet terminals are also connected to it. Now, terminate the server program. All Telnet terminals will show Connection closed by foreign host.

## Handling multiple connections

To handle every connection, we need separate handling code to run along with the main server thread, which accepts incoming connection requests. One way to achieve this is to use threads. The main server program accepts an incoming connection request and provisions a new thread to handle communication for the connection, and then, the server goes back to accept more incoming connection requests.

This is the required code:

```
import socket
import sys
from thread import *

HOST = ''  # Symbolic name meaning all available interfaces
PORT = 8888 # Arbitrary non-privileged port

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
print 'Socket created'

#Bind socket to local host and port
try:
    s.bind((HOST, PORT))
except socket.error , msg:
    print 'Bind failed. Error Code : ' + str(msg[0]) + ' Message '
    + msg[1]
    sys.exit()

print 'Socket bind complete'
```

```
#Start listening on socket
s.listen(10)
print 'Socket now listening'

#Function for handling connections. This will be used to create
threads
def clientthread(conn):
    #Sending message to connected client
    conn.send('Welcome to the server. Type something and hit\nenter\n') #send only takes string

    #infinite loop so that function do not terminate and thread do
    not end.
    while True:

        #Receiving from client
        data = conn.recv(1024)
        reply = 'OK...' + data
        if not data:
            break

        conn.sendall(reply)

    #came out of loop
    conn.close()

#now keep talking with the client
while 1:
    #wait to accept a connection - blocking call
    conn, addr = s.accept()
    print 'Connected with ' + addr[0] + ':' + str(addr[1])

    #start new thread takes 1st argument as a function name to be
    #run, second is the tuple of arguments to the function.
    start_new_thread(clientthread ,(conn,))

s.close()
```

Run this server code and open three terminals like before. Now, the server will create a thread for each client connecting to it.

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

## Looking back

By now, we have learned the basics of socket programming in Python. When testing these programs, if you face the following error, simply change the port number, and the server will run fine:

```
Bind failed. Error Code : 98 Message Address already in use
```

## A Telnet client in Python

The Telnet client is a simple command-line program that is used to connect to socket servers and exchange messages. The following is an example of how to use Telnet to connect to https://www.google.com and fetch the homepage:

```
$ telnet www.google.com 80
```

This command will connect to www.google.com on port 80.

```
$ telnet www.google.com 80
Trying 74.125.236.69...
Connected to www.google.com.
Escape character is '^]'
```

Now that it is connected, the Telnet command can take user input and send it to the respective server, and whatever the server replies will be displayed in the terminal. For example, send the HTTP GET command in the following format and hit Enter twice:

```
GET / HTTP/1.1
```

Sending this will generate a response from the server. Now we will make a similar Telnet program. The program is simple: we will implement a program that takes user input and fetches results from the remote server in parallel using the threads. One thread will keep receiving messages from the server and another will keep taking in user input. But there is another way to do this apart from threads: the select() function. This function allows you to monitor multiple sockets or streams for readability and will generate an event if any of the sockets are ready.

The code for it is as follows:

```
import socket, select, string, sys

#main function
if __name__ == "__main__":
    if(len(sys.argv) < 3) :
        sys.exit()
    host = sys.argv[1]
    port = int(sys.argv[2])
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.settimeout(2)
    # connect to remote host
    try :
        s.connect((host, port))
    except :
        print 'Unable to connect'
        sys.exit()
```

## The chat server

The chat server performs the following tasks:

- It accepts multiple incoming connections for the client.
- It reads incoming messages from each client and broadcasts them to all the other connected clients.

The following is the code for the chat server:

```
import socket, select

#Function to broadcast chat messages to all connected clients
def broadcast_data (sock, message):
    #Do not send the message to master socket and the client who
    has send us the message
    for socket in CONNECTION_LIST:
        if socket != server_socket and socket != sock :
            try :
                socket.send(message)
            except :
                # broken socket connection may be, chat client
                pressed ctrl+c for example
                socket.close()
                CONNECTION_LIST.remove(socket)

if __name__ == "__main__":

    # List to keep track of socket descriptors
    CONNECTION_LIST = []
    RECV_BUFFER = 4096 # Advisable to keep it as an exponent of 2
    PORT = 5000

    server_socket = socket.socket(socket.AF_INET,
    socket.SOCK_STREAM)
    # this has no effect, why ?
    server_socket.setsockopt(socket.SOL_SOCKET,
    socket.SO_REUSEADDR, 1)
    server_socket.bind(("0.0.0.0", PORT))
    server_socket.listen(10)

    # Add server socket to the list of readable connections
    CONNECTION_LIST.append(server_socket)

    print "Chat server started on port " + str(PORT)
```

```
while 1:
    # Get the list sockets which are ready to be read through
select
    read_sockets,write_sockets,error_sockets =
    select.select(CONNECTION_LIST,[],[])

    for sock in read_sockets:
        #New connection
        if sock == server_socket:
            # Handle the case in which there is a new
            connection recieved through server_socket
            sockfd, addr = server_socket.accept()
            CONNECTION_LIST.append(sockfd)
            print "Client (%s, %s) connected" % addr

            broadcast_data(sockfd, "[%s:%s] entered room\n" %
            addr)

        #Some incoming message from a client
        else:
            # Data received from client, process it
            try:
                #In Windows, sometimes when a TCP program
                closes abruptly,
                # a "Connection reset by peer" exception will
                be thrown
                data = sock.recv(RECV_BUFFER)
                if data:
                    broadcast_data(sock, "\r" + '<' +
                    str(sock.getpeername()) + '> ' + data)

            except:
                broadcast_data(sock, "Client (%s, %s) is
                offline" % addr)
                print "Client (%s, %s) is offline" % addr
                sock.close()
                CONNECTION_LIST.remove(sock)
                continue

server_socket.close()
```

In this code, the server program opens up port 5000 to listen for incoming connections from the clients. The chat client must connect to the same port. We can change the port number if we want by specifying another number that is not used by any other program or process.
