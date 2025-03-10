
[Code](https://github.com/freth251/http-from-scratch/tree/main) 

## client server model

Every network application is based on the client server model. 

![[client-server.png]]

In brief: 
1. the client program sends a request to the server program
2. the server program processes this requests
3. the server sends back a response to the client program
4. the client processes this request. 

This is what happens each time you connect to the internet. This is the blueprint for all network communication. 

## the sockets interface

![[computer-view.png]]


In Unix, and unix like systems, all I/O devices are modelled as files (sequence of bytes). The keyboard is a file, the headphone connected to your computer is a file, the monitor connected to it is a file. To write to an I/O device, you write to a file, and to read from an I/O device, you read from a file. 

Network adapters are the same. When we want to send a request to a server, we write it to a file and when we get back a response we read a from a file. 

This is what sockets are. They are functions provided to us by the operating system, aka system calls, we use to create and configure the files used to write to and read from I/O devices. 

In addition from creating the files, through the socket interface, the kernel also wraps our message in the protocol of our choice, here TCP/IP, so that we can correctly address our client or server. 

![[global-ip.png]]

Depending on if we are creating a server or a client, the system calls we use are different. Here's an overview of the function calls needed for a client and a server to communicate: 
![[socket-api.png]]

### client side: 

First things first, we need to retrieve the server's address. We can do this in various ways. We can either use the `getaddrinfo` function to perform a DNS lookup for a specific node (hostname) and service (http/ssh or the port number). Or we can use the `gethostbyname` function to get the server's address via the hostname. 


```c
#include <netdb.h>

int getaddrinfo(const char *restrict node,
			   const char *restrict service,
			   const struct addrinfo *restrict hints,
			   struct addrinfo **restrict res);

struct hostent *gethostbyname(const char *name);
```

We then create the socket. As mentioned the socket is just a file, and like creating any file the kernel returns it corresponding descriptor. 

```c
#include <sys/socket.h>

int socket(int domain, int type, int protocol);
```

- the `domain` is used to specify the protocol (we will use )
- the `type` indicates the type of communication (streams/datagrams or raw)
- the `protocol` idk what this does. 
- it returns a file descriptor. 

The socket function creates the file but it is not yet connected to anything. 

That is where the `connect` function comes. Using the socket we created above and the server's address we obtained, we attempt to send a connection request to the server. 

```c
#include <sys/types.h>
#include <sys/socket.h>

int connect(int socket, const struct sockaddr *address, socklen_t address_len);
```

- `socket` is the return value of the socket function
- `address` is the server address obtained through the `getaddrinfo`or `gethostbyname` functions
- `address_len` is the length of `address`

Once we have done the steps above we can send and receive messages through the `send` and `recv` system calls. 

```c
#include <sys/socket.h>

ssize_t send(int socket, const void *buffer, size_t length, int flags);
```
- `socket`: the socket descriptor created by the socket function
- `buffer`: the message we want to send
- `length`: length of buffer
- `flags`: not used

```c
 #include <sys/socket.h>

ssize_t recv(int socket, void *buffer, size_t length, int flags);
```
- `socket`: the socket descriptor created by the socket function
- `buffer`: the message we want to send
- `length`: length of buffer
- `flags`: not used





When  we are done we can close the socket using `close`. 

Here's a code snippet for creating a client and connecting to a server: 

```c
int create_client(char * hostname, int port){

	struct hostent *hp;
	struct sockaddr_in serveraddr;
	int sockfd= socket(AF_INET, SOCK_STREAM, 0 );
	if(sockfd<0){
		printf("Error creating socket");
		return -1;
	}
	
	if ((hp= gethostbyname(hostname))==NULL){
		printf("Error getting host");
		return -1;
	}
	
	bzero((char *) &serveraddr, sizeof(serveraddr));
	serveraddr.sin_family = AF_INET;
	bcopy((char *)hp->h_addr_list[0],
	(char *)&serveraddr.sin_addr.s_addr, hp->h_length);
	serveraddr.sin_port = htons(port);
	
	if (connect(sockfd, (struct sockaddr *)&serveraddr, sizeof(serveraddr))<0){
		printf("Error connecting to server");
		return -1;
	}
	
	return sockfd;
}

```


### Server side 

Now let's hop on the server side. We create the server socket the same way we created the client's socket. Depending on our use case we may or may not need to use the `getaddrinfo` function. If we accept all connections associated with this host then we don't use it, if we only accept connections addressed to specific IP addresses then we can use it to look up what the IP addresses associated to this host is. 

After we create the socket descriptor, we need to bind it to the server's address. We do so by calling the `bind` function: 

```c
#include <sys/socket.h>

int bind(int socket, const struct sockaddr *address, socklen_t address_len);
```
- `socket`: the server's socket descriptor
- `address`: the server's address
- `address_len`: the size of address

Then we pass the socket descriptor into the `listen` function to convert it from an *active* socket to a *listening* socket. 

```c
#include <sys/socket.h>  
int listen(int sockfd, int backlog);
```
- `sockfd`: the server's socket descriptor
- `backlog`: the number of connections to queue up before refusing connections

Here's a code snippet putting all this together: 

```c
int create_server(int port){
    
    int sockfd= socket(AF_INET, SOCK_STREAM, 0 );

    if(sockfd==-1){
        printf("Error creating socket");
        return -1;
    }

    struct sockaddr_in serveraddr;

    serveraddr.sin_family= AF_INET;

    serveraddr.sin_addr.s_addr=htonl(INADDR_ANY);

    serveraddr.sin_port= htons((unsigned short)port);

    if (bind(sockfd, (struct sockaddr *)&serveraddr, sizeof(serveraddr))<0){
        printf("Error binding socket");
        return -1;
    }

    if (listen(sockfd, BACKLOG_LEN)<0){
        printf("Error listening socket");
        return -1;
    }

    return  sockfd;
}```

The server now waits for connections using the `accept` function. This function blocks the process until a connection request arrives from the client and returns a client descriptor once it does. The client descriptor is different then the socket descriptor we created above and is newly created for each incoming connections. This way the server can listen to many connections via the same socket descriptor but do the communication through a new descriptor. 

```c
#include <sys/socket.h>

int accept(int listenfd, struct sockaddr *addr, int *addrlen);
```

Once we receive a connection request we can use the  same `send` and `receive` functions to communicate with the client. 


## http web server

HTTP is an application protocol used to send HTML files. It is an application layer protocol meaning it dictates the behavior of the data we send between client and server.

Once we have established a server that is capable of accepting connections, we need to make sure that it is a able to process and send back data that complies with the http protocol. The web server can either serve static content, a page that is prebuilt during development, or dynamic content, a page that is built on a per request basis, by executing some code on the server. 

### http request

http request have a request line followed by zero or more request header. 

the request line has the following fields: 

\<method\> <\uri\> <\version\>

- method: this indicates the operation that the client wants us to perform, can be GET, POST, OPTIONS, HEAD, PUT, DELETE, and TRACE. For our implementation we will only handle GET requests. 
- uri: this the Uniform Resource Identifier, it indicates which file/resource the client wants us to process. 
- version: indicates the http version. 

the request header has the following fields: 
<\header name>: <\header data>

### http response

http responses have a response line followed by zero or more response headers followed by zero or more response bodies. 

response have the following fields: 
<\version>  <\status code> <\status message>

a full list of status codes and status messages can be found [here](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes).

the response body is where we have the html file that is displayed on our browser. 


## static content

To display static content, all we need to do is copy the file specified by the uri into the body of the http response. 

## dynamic content

To generate dynamic content, the server creates a child process to execute some code and waits for it to finish. The common gateway interface standard defines how the server process passes information to child process and where the child process sends its outputs. 






