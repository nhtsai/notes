---
layout: post
title: "Building a Web Server"
description: "Learning how to build a basic webserver."
author: "Nathan Tsai"
toc: true
comments: false
hide: false
search_exclude: false
show_tags: true
categories: [web-server]
permalink: /web-server
---

# Title
* https://ruslanspivak.com/lsbaws-part1/
* https://joaoventura.net/blog/2017/python-webserver/


## Introduction
* A webserver is a networking server that waits for a client to send a request and generates a response to send back to the client.
* The communication between a client and server uses HTTP protocol.
* The client can be a browser or any other software that uses HTTP.
* HTTP is just text that follows a certain pattern.
    * Specify which resource and its headers
    * Separate head and body with blank space
    * Specify body message
* A socket is an abstraction that allows you to send and receive bytes through a network.

* Example code in python
    ```python
    # Python3.7+
    import socket

    # define socket host and port
    HOST, PORT = '', 8888

    # Create socket
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
    server_socket.bind((HOST, PORT))
    server_socket.listen(1)
    print(f'Serving HTTP on port {PORT} ...')

    while True:
        # Wait for client connections
        client_connection, client_address = server_socket.accept()

        # Get client request
        request_data = client_connection.recv(1024)
        print(request_data.decode('utf-8'))

        # Send HTTP response
        http_response = b"HTTP/1.1 200 OK\n\nHello, World!"
        client_connection.sendall(http_response)
        client_connection.close()

    server_socket.close()
    ```
    * We first define server socket's host and port.
    * Then we create a server socket and set it to `AF_INET` for IPv4 address family and `SOCK_STREAM` for TCP.
    * This webserver creates a socket and binds it to port 8888 of any available host (localhost) to listen for connections.
    * The server socket accepts a connection and gets a client socket to receive information and the address of the client socket on the other side of the connection.
    * The server receives 1,024 bytes of data from the client socket.
    * The server then sends a message in bytes to the client socket.
    * Finally, the client closes the client socket connection.

* URL `http://localhost:8888/hello`
    * `http` designates HTTP protocol
    * `localhost` is the host name
    * `8888` is the port number
    * `/hello` is the path, or page to connect

* Before the client can send HTTP requests, a TCP connection with the web server must be established.
* Then the client sends an HTTP request and receives a response from the web server.

* HTTP Request `GET /hello HTTP/1.1`
    * `GET` is the HTTP method
    * `/hello` is the path, or page to connect
    * `HTTP/1.1` is the protocol version

* HTTP Response

    ```
    HTTP/1.1    200 OK

    Hello, World!
    ```

    * `HTTP/1.1` is the protocol version
    * `200 OK` is the HTTP status code
    * `Hello, World!` is the HTTP response body


* By default the browser requests the root of a server, e.g. `GET / HTTP/1.0`, but we should return the `index.html` page instead.

    ```python
    while True:
        # Wait for client connections
        client_connection, client_address = server_socket.accept()

        # Get client request
        request_data = client_connection.recv(1024)
        print(request_data.decode('utf-8'))

        # Get the contents of htdocs/index.html
        fin = open('htdocs/index.html')
        content = fin.read()
        fin.close()

        # Send HTTP response with content from index.html
        # Encode entire message to bytes
        http_response = "HTTP/1.1 200 OK\n\n" + content
        client_connection.sendall(http_response.encode())
        client_connection.close()
    ```

## References
[^1]: Footnote