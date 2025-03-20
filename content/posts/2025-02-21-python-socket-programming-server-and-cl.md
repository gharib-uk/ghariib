---
title: "Python Socket Programming Server and Client Example Guide"
date: 2025-02-21T00:00:00.000Z
draft: false
type: posts
categories: 
- python
---
# Python Socket Programming Server and Client Example Guide

<br/>

<br/>
### [Introduction](#introduction)[](#introduction)

Python’s socket module is a powerful tool for creating network applications. In this tutorial, you will learn the basics of [Python socket programming](/community/tutorials/what-is-a-socket), including how to create a simple client-server architecture, handle multiple clients using threading, and understand the differences between [TCP and UDP sockets](/community/tutorials/understanding-sockets#what-is-a-stream-socket). You will learn how to establish connections, send and receive data, and build robust network applications using Python.

[What is Socket Programming in Python?](#what-is-socket-programming-in-python)[](#what-is-socket-programming-in-python)
-----------------------------------------------------------------------------------------------------------------------

To understand [Python socket](/community/tutorials/what-is-a-socket) programming, we need to know about three interesting topics - **Socket Server**, **Socket Client**, and **Socket**.

So, what is a server? Well, a server is software that waits for client requests and serves or processes them accordingly.

On the other hand, a client is a requester of this service. A client program requests some resources from the server, and the server responds to that request.

The socket is the endpoint of a bidirectional communications channel between the server and the client. Sockets may communicate within a process, between processes on the same machine, or between processes on different machines. For any communication with a remote program, we have to connect through a socket port. The main objective of this socket programming tutorial is to familiarize you with how socket servers and clients communicate with each other. You will also learn how to write a Python socket server program.

[Python Socket Example](#python-socket-example)[](#python-socket-example)
-------------------------------------------------------------------------

We have said earlier that a socket client requests for some resources to the socket server and the server responds to that request. So we will design both server and client model so that each can communicate with them. The steps can be considered like this.

1.  Python socket server program executes at first and waits for any request
2.  Python socket client program will initiate the conversation at first.
3.  Then server program will response accordingly to client requests.
4.  Client program will terminate if user enters “bye” message. Server program will also terminate when client program terminates, this is optional and we can keep server program running indefinitely or terminate with some specific command in client request.

### [Python Socket Server](#python-socket-server)[](#python-socket-server)

We will save the Python socket server program as `socket_server.py`. To use [python socket connection](/community/tutorials/understanding-sockets), we need to import **socket** module. Then, sequentially we need to perform some task to establish connection between server and client. We can obtain host address by using `socket.gethostname()` function. It is recommended to user port address above 1024 because port number lesser than 1024 are reserved for standard internet protocol. See the below python socket server example code, the comments will help you to understand the code.

```
import socket


def server_program():
    # get the hostname
    host = socket.gethostname()
    port = 5000  # initiate port no above 1024

    server_socket = socket.socket()  # get instance
    # look closely. The bind() function takes tuple as argument
    server_socket.bind((host, port))  # bind host address and port together

    # configure how many client the server can listen simultaneously
    server_socket.listen(2)
    conn, address = server_socket.accept()  # accept new connection
    print("Connection from: " + str(address))
    while True:
        # receive data stream. it won't accept data packet greater than 1024 bytes
        data = conn.recv(1024).decode()
        if not data:
            # if data is not received break
            break
        print("from connected user: " + str(data))
        data = input(' -> ')
        conn.send(data.encode())  # send data to the client

    conn.close()  # close the connection


if __name__ == '__main__':
    server_program()
```

So our python socket server is running on port 5000 and it will wait for client request. If you want the server to not quit when the client connection is closed, just remove the [if condition](/community/tutorials/python-if-else-elif) and `break` the statement. [Python while loop](/community/tutorials/python-while-loop) is used to run the server program indefinitely and keep waiting for client request.

### [Python Socket Client](#python-socket-client)[](#python-socket-client)

We will save python socket client program as `socket_client.py`. This program is similar to the server program, except binding. The main difference between server and client program is, in server program, it needs to bind host address and port address together. See the below python socket client example code, the comment will help you to understand the code.

```
import socket


def client_program():
    host = socket.gethostname()  # as both code is running on same pc
    port = 5000  # socket server port number

    client_socket = socket.socket()  # instantiate
    client_socket.connect((host, port))  # connect to the server

    message = input(" -> ")  # take input

    while message.lower().strip() != 'bye':
        client_socket.send(message.encode())  # send message
        data = client_socket.recv(1024).decode()  # receive response

        print('Received from server: ' + data)  # show in terminal

        message = input(" -> ")  # again take input

    client_socket.close()  # close the connection


if __name__ == '__main__':
    client_program()
```

### [Python Socket Programming Output](#python-socket-programming-output)[](#python-socket-programming-output)

To see the output, first run the socket server program. Then run the socket client program. After that, write something from client program. Then again write reply from server program. At last, write **bye** from client program to terminate both program. Below short video will show how it worked on my test run of socket server and client example programs. ![python socket programming, python socket server](https://journaldev.nyc3.cdn.digitaloceanspaces.com/2017/09/python-socket-example.gif)

```
python3.6 socket_server.py 
Connection from: ('127.0.0.1', 57822)
from connected user: Hi
 -> Hello
from connected user: How are you?
 -> Good
from connected user: Awesome!
 -> Ok then, bye!
```

```
python3.6 socket_client.py 
 -> Hi
Received from server: Hello
 -> How are you?
Received from server: Good
 -> Awesome!
Received from server: Ok then, bye!
 -> Bye
```

Notice that socket server is running on port 5000 but client also requires a socket port to connect to the server. This port is assigned randomly by client connect call. In this case, it’s `57822`.

[Handling Multiple Clients Using Threads](#handling-multiple-clients-using-threads)[](#handling-multiple-clients-using-threads)
-------------------------------------------------------------------------------------------------------------------------------

A server can utilize [threads](https://docs.python.org/3/library/threading.html) to handle multiple clients simultaneously. This approach allows the server to process multiple client connections concurrently, enhancing its responsiveness and ability to handle a large number of clients efficiently.

Here’s an example of how this can be achieved in Python using the [`threading` module](https://docs.python.org/3/library/threading.html):

```
import threading

def handle_client(client_socket):
    # This function is responsible for handling each client connection.
    # It sends a greeting message to the client and then closes the connection.
    client_socket.send(b"Hello, Client!")
    client_socket.close()

while True:
    # The server continuously listens for incoming client connections.
    client_socket, addr = server_socket.accept()
    # When a new client connects, a new thread is created to handle the client.
    client_thread = threading.Thread(target=handle_client, args=(client_socket,))
    client_thread.start()
```

[Blocking vs. Non-Blocking Sockets](#blocking-vs-non-blocking-sockets)[](#blocking-vs-non-blocking-sockets)
-----------------------------------------------------------------------------------------------------------

In socket programming, sockets can operate in either [blocking](https://docs.python.org/3/howto/sockets.html#) or [non-blocking](https://docs.python.org/3/howto/sockets.html#non-blocking-sockets) mode. This mode determines how the socket behaves when it is waiting for data to be received or sent.

-   **Blocking Sockets**: In blocking mode, operations like `recv()` and `accept()` will block the execution of the program until data is available. This means that the program will pause and wait for data to be received or a connection to be accepted. While this approach is simple to implement, it can lead to performance issues and unresponsiveness in applications that need to handle multiple tasks concurrently.
    
-   **Non-Blocking Sockets**: Non-blocking sockets, on the other hand, return immediately if no data is available. This approach prevents the application from freezing or becoming unresponsive due to socket operations. Non-blocking sockets are particularly useful in applications that need to handle multiple connections simultaneously, such as web servers or chat servers. However, they require more complex programming to handle the asynchronous nature of the operations.
    

[TCP vs. UDP Sockets](#tcp-vs-udp-sockets)[](#tcp-vs-udp-sockets)
-----------------------------------------------------------------

Feature

TCP (Transmission Control Protocol)

UDP (User Datagram Protocol)

Connection

Connection-oriented

Connectionless

Reliability

Reliable, ensures data integrity

Unreliable, no guarantee

Use Cases

Web browsing, file transfer

Video streaming, gaming

[TCP (Transmission Control Protocol)](https://en.wikipedia.org/wiki/Transmission_Control_Protocol) and [UDP (User Datagram Protocol)](https://en.wikipedia.org/wiki/User_Datagram_Protocol) are two fundamental protocols in the internet protocol suite that enable communication over the internet. The key differences between them lie in their approach to establishing connections and ensuring data reliability.

TCP is a connection-oriented protocol, which means a connection is established between the sender and receiver before data is sent. This ensures that data packets are delivered in the correct order and retransmitted if lost or corrupted, ensuring data integrity. TCP is suitable for applications that require guaranteed delivery of data, such as web browsing and file transfer.

On the other hand, UDP is a connectionless protocol, which means there is no dedicated end-to-end connection. Data is sent as a series of packets, and the receiver does not send an acknowledgement. This makes UDP faster but less reliable than TCP. UDP is commonly used for applications that require fast transmission and can tolerate some loss of data, such as video streaming and gaming.

[Common Errors and Debugging](#common-errors-and-debugging)[](#common-errors-and-debugging)
-------------------------------------------------------------------------------------------

1.  `Address Already in Use Error`: This error occurs when a process is already using the port you’re trying to use. To resolve this, you can use the following command to identify the process using the port:
    
    ```
    netstat -an | grep <port_no>
    ```
    
    Once you’ve identified the process, you can either stop the process or use a different port for your application.
    
2.  `Connection Refused Error`: This error occurs when the client attempts to connect to a server that is not running or not listening on the specified port. To resolve this, ensure that the server is running and listening on the correct port before the client attempts to connect. Here’s a simple Python code block to check if a server is listening on a specific port:
    

````
import socket

def check_port(host, port):
    try:
        with socket.create_connection((host, port), timeout=1):
            print(f"Port {port} is open on {host}")
    except socket.timeout:
        print(f"Port {port} is not open on {host}")

# Example usage
check_port('localhost', 5000)

3. Exception Handling: It's essential to handle exceptions properly to ensure your application can recover from errors gracefully. Here's an example of how to handle a connection error using a `try-except` block:

```python
try:
    client_socket.connect(('localhost', 12345))
except socket.error as e:
    print(f"Error connecting to the server: {e}")
    # Optionally, you can add additional error handling or logging here
except Exception as e:
    print(f"An unexpected error occurred: {e}")
    # Optionally, you can add additional error handling or logging here
````

[FAQs](#faqs)[](#faqs)
----------------------

### [1\. How to create a Python socket server?](#1-how-to-create-a-python-socket-server)[](#1-how-to-create-a-python-socket-server)

Here is an example of how to create a Python socket server:

```
import socket

def server_program():
    # get the hostname
    host = socket.gethostname()
    port = 5000  # initiate port no above 1024

    server_socket = socket.socket()  # get instance
    # look closely. The bind() function takes tuple as argument
    server_socket.bind((host, port))  # bind host address and port together

    # configure how many clients the server can listen to simultaneously
    server_socket.listen(2)
    conn, address = server_socket.accept()  # accept new connection
    print("Connection from: " + str(address))
    while True:
        # receive data stream. it won't accept data packets greater than 1024 bytes
        data = conn.recv(1024).decode()
        if not data:
            # if data is not received break
            break
        print("from connected user: " + str(data))
        data = input(' -> ')
        conn.send(data.encode())  # send data to the client

    conn.close()  # close the connection


if __name__ == '__main__':
    server_program()
```

### [2\. How to create a Python socket client?](#2-how-to-create-a-python-socket-client)[](#2-how-to-create-a-python-socket-client)

```
import socket

def client_program():
    host = socket.gethostname()  # as both code is running on same pc
    port = 5000  # socket server port number

    client_socket = socket.socket()  # instantiate
    try:
        client_socket.connect((host, port))  # connect to the server
    except ConnectionRefusedError:
        print("The connection to the server was refused")
        return

    message = input(" -> ")  # take in the user's message

    while message.lower().strip() != 'bye':
        client_socket.send(message.encode())  # send in the socket and in networking
        data = client_socket.recv(1024).decode()  # receive in the client
        print('Response from the server : ' + data)  # in the client console
        message = input(" -> ")

    client_socket.close()  # close the connection

if __name__ == '__main__':
    client_program()
```

### [3\. What is the difference between TCP and UDP sockets?](#3-what-is-the-difference-between-tcp-and-udp-sockets)[](#3-what-is-the-difference-between-tcp-and-udp-sockets)

TCP provides reliable communication, whereas UDP is faster but does not guarantee data integrity.

Feature

TCP

UDP

Connection

Connection-oriented

Connectionless

Reliability

Guaranteed delivery

No guarantee of delivery

Ordering

Ensures correct order

No guarantee of order

Error-checking

Error-checking and correction

Error-checking but no correction

Speed

Slower due to error-checking

Faster due to no error-checking

Use Cases

File transfer, email, web browsing

Online gaming, video streaming, VoIP

### [4\. How to handle multiple clients in Python socket programming?](#4-how-to-handle-multiple-clients-in-python-socket-programming)[](#4-how-to-handle-multiple-clients-in-python-socket-programming)

You can use threading to manage multiple clients concurrently. Here’s an example of how you can modify the server code to handle multiple clients using threading:

```
import threading

def handle_client(conn, addr):
    print(f"New connection from {addr}")
    while True:
        try:
            message = conn.recv(1024).decode()
            if message:
                print(f"Message from {addr}: {message}")
                conn.send(message.encode())
        except Exception as e:
            print(f"Error with connection from {addr}: {e}")
            break
    conn.close()

def server_program():
    host = socket.gethostname()
    port = 5000

    server_socket = socket.socket()
    server_socket.bind((host, port))
    server_socket.listen()

    while True:
        conn, addr = server_socket.accept()
        thread = threading.Thread(target=handle_client, args=(conn, addr))
        thread.start()
        print(f"Active connections: {threading.activeCount() - 1}")

if __name__ == '__main__':
    server_program()
```

[Conclusion](#conclusion)[](#conclusion)
----------------------------------------

In this tutorial, you learned the basics of Python socket programming, including how to create a simple client-server architecture, handle multiple clients using threading, and understand the differences between TCP and UDP sockets. To further enhance your knowledge of Python programming, we recommend checking out the following tutorials:

1.  [Python Tutorial](/community/tutorials/python-tutorial)
2.  [What is a Socket?](/community/tutorials/what-is-a-socket)
3.  [Understanding Sockets](/community/tutorials/understanding-sockets)

These tutorials will provide you with a deeper understanding of Python and its applications in network programming.

#### [Source](https://www.digitalocean.com/community/tutorials/python-socket-programming-server-client)

<br/>
---
