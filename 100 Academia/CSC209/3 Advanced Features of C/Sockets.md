---
dg-publish: true
tags: [cs, lecture, note, university]
Course Code:
  - "[[CSC209]]"
aliases: []
Week: 10
Module:
  - "[[3 - Advanced Features of C]]"
Lecture:
  - "17"
Chapter: 
Slides/Notes: 
Date: 2025-03-18
Date created: Mon., Mar. 17, 2025, 6:26:23 pm
Date modified: Wed., Mar. 19, 2025, 11:29:33 am
---

# Sockets

## Introduction to Sockets

> [!info]+ So far:
> - Have learned how to communicate between processes using [[Pipes|pipes]] and [[Signals|signals]]
> - To communicate using pipes:
>     - Processes have to be created using [[Process Models|forking]]
> - Signals can be used to communicate between unrelated processes
> - However:
>     - & Both pipes and signals can only be used to communicate between processes that are on the same machine

> [!goal] How can we use sockets to communicate between two processes running on two *different* machines?

First we need to know just a bit about the **internet**.

- & Each machine has an **internet protocol**
    - i.e., **IP address**
    - An address that can be used to send a message to it from any other machine that is connected to the internet

![](https://i.imgur.com/TnAVfIo.png)

- Machine might be running lots of different programs that need to communicate over the internet
    - Although a machine might have only one IP address
- Messages sent across internet are destined for a particular *program*
    - → Need more than just machine to specify which program

![](https://i.imgur.com/xnmfJQF.png)

- **Ports**
    - Think of ports like an apartment number in building at that address
        - Machine’s address is like a street address
- Full location of a program running on a machine connected to internet
    - Machine *address* plus *port*

![](https://i.imgur.com/8NMg3Uv.png)

- Messages sent from one machine are enclosed in **packets**
    - Think of them as packages
    - Contain both *address* and **payload**
        - Contents of the packet or package
- ! Packet does not specify the *route* that it travels to get to the destination
    - Route is determined as the packet moves
- When packet leaves machine:
    - **Router** receives the packet

![](https://i.imgur.com/DMBnIGz.png)

- **Router**
    - Facilitates transfer of packets between networks
    - Connected to multiple networks
    - Know which network the packet should be sent to in order to get it closer to its final destination

![](https://i.imgur.com/qmBAles.png)

- Destination machine looks at final destination
    - Sends the packet onto the next stage of its journey

### Client and Server

![|400](https://i.imgur.com/Q43jlU4.png)

- **Server**
    - Program running on a specific port of a certain machine
        - Machine is waiting for another program (or maybe many others) to send a message

> [!note]+ Many common services have defined ports.
> - **Webpages**
>     - Typically served on port `80`
> - **Secure web pages**
>     - Port `443`

![](https://i.imgur.com/BYb9zRK.png)

- In other cases:
    - Person running the server publishes the machine *address* and *port*
        - “If you want to run my program, or play my game, or use my service, then send the data to me at this address (e.g., `195.67.7.8`) and this port (e.g., `30001`). I will be sitting here waiting for messages whenever you are ready”

![](https://i.imgur.com/PcmA3bW.png)

- & User runs a **client** program when they want to start interacting with a *server*
    - Client program sends the initial message
- In some cases:
    - Client sends only a *single* message
- In other cases:
    - Client begins a **connection**
        - Conversation between the two machines that involves multiple messages
- Client sends first message to initialize the connection
- Once programs have established a communication channel:
    - Either machine can send data to the other

![](https://i.imgur.com/Q22bdrs.png)

> [!question]+ How will we establish a communication channel?
> - **Sockets**

> [!summary]+ Types of sockets
> 1. Datagram sockets
> 2. Stream sockets
> 3. Raw sockets
> 4. … other

- Different types all rely on the same system calls
    - → System calls have many different options to allow you to set up the kind of socket you want
- % We only demonstrate one kind of socket: **stream sockets**
    - Build on the **TCP protocol**
    - Connection-oriented sockets
    - No loss guaranteed (in transit)
    - In-order delivery

### `socket`

```c
int socket(int domain, int type, int protocol)
```

- `socket`
    - Creates an endpoint for communication
    - System call
- Set up:
    - One endpoint in client program
    - One endpoint in server
    - & Both programs will independently invoke this system call
- Return value:
    - Type `int`
    - Returns `index` of an entry in the file-descriptor table
    - `-1` if there is an error

<!-- break -->
- `domain`
    - Sets the protocol (or set of rules) used for communication
    - Can set to defined constant `PF_INET` or `AF_INET`
        - Since communicating over the internet
        - % `PF_INET` and `AF_INET` are defined the same in `socket.h`
            - For historical reasons
            - Original designed expected that an address family (`AF`) might include a number of protocol families (`PF`), but that never happened

<!-- break -->
- `type`
    - Use stream sockets
        - `SOCK_STREAM`

<!-- break -->
- `protocol`
    - Configures which protocol the socket will use for communication
    - % Stream sockets use the TCP protocol
        - → Set `protocol` to `0`
            - Tells socket system call to use default protocol for this type of socket

```c
int socket(AF_INET, SOCK_STREAM, 0);
```

- & *Client* and *server* programs will call `socket` like this
    - → Create a socket endpoint
- File descriptor returned by the function will be used by the system calls that establish a connection

## Socket Configuration

In [[#Introduction to Sockets]], we saw one way that we could set up an *endpoint* for communication between two processes.

- Used `socket` system call to create a *stream* socket

> [!goal] Configure that socket to *wait* for connections at a specific address

```c
int listen_soc = socket(AF_INET, SOCK_STREAM, 0);
if (listen_soc == -1) {
    perror("socket");
    exit(1);
}
```

So far, we have created a socket like this.

- Sets up our stream socket, but
    - ! Does not set the address!
    - & Use `bind` system call for this

### `bind`

```c
int bind(int socket, const struct sockaddr *address, socklen_t address_len);
```

- $ `bind` takes *three* parameters
- `socket`
    - Socket that we want to configure
        - The one we just created (`listen_soc`)
- `address`
    - & Type *pointer* to `struct sockaddr`
        - `bind` works for all different address families
            - % Type is *generic* → for all the families
        - But for our particular family `AF_INET`:
            - Will set this parameter by using a `struct sockaddr_in`
            - `in` for “internet”
                - Not “in” vs. out

> [!impl]+ First, we create the struct we need.

#### Definition of `struct sockaddr_in`

```c
struct sockaddr_in {
    short sin_family;
    u_short sin_port;
    struct in_addr sin_addr;
    char sin_zero[8];
}
```

- `sin_family`
    - Use `AF_INET`
        - Consistent with our socket

##### Ports

- `sin_port`
    - Set the *port number*
    - % Port numbers range from 0 to just over 65,000
        - **Well-reserved ports**
            - Lowest 0-1023 are reserved for well-known services
            - e.g., Telnet — old program that allows users to remotely access another machine
                - Runs on port 23
        - **Registered ports**
            - Ports 1024-49,151
            - If you use these ports for a service you wish to make public, you can register with IANA
                - Internet Assigned Numbers Authority
                - IANA looks after assigning domain names at the highest level
        - **Dynamic ports**
            - 49,152-65,535
            - If you are writing a server to run on your own machine, any port over 49,151 will be fine
            - If you are writing a server to run on a shared machine, do not want to set up socket on a port that some other program is already using
                - e.g., one of your classmate’s
                - Need to work out a plan for who uses which port

![](https://i.imgur.com/9JsJe8J.png)

Suppose your instructor told you to use port 54321 for a course project.

![](https://i.imgur.com/VeEsXsA.png)

- $ Extra call to function `htons`
- **==`htons`==**
    - Host-to-network short
    - In memory:
        - Storing an integer takes more than one byte
        - & Different machines store the bytes that make up that integer in different orders
- When we write network code:
    - & Have to make sure that the two machines are communicating and speaking with the *same language*

![](https://i.imgur.com/l0EwY5m.png)

- Have to *transmit* (and *expect*) particular data in a *specific format*
    - Agreements are called **protocols**
        - Essential when we communicate between different programs particular over a network

![](https://i.imgur.com/A0qcAV6.png)

- & Need to make sure that the integers we send are in the correct format

![|400](https://i.imgur.com/36YKd6b.png)

- Use `htons` to convert integer from the byte order of the host machine to what the *protocol* says
- If machine compiling and running this code already uses network byte order:
    - `htons` will do nothing

![|300](https://i.imgur.com/sRLRGm1.png)

- If same code is compiled on a machine that does not use what protocol specifies as network byte order:
    - Function will *flip* the bytes around as needed

![](https://i.imgur.com/VeEsXsA.png)

- $ Field of `struct sockaddr_in` that holds *machine address* is actually a struct itself
    - `sin_addr`
        - Type `struct in_addr`

![](https://i.imgur.com/5HCEveh.png)

- & Only field of this inside struct that we need to set is `s_addr`
    - Use predefined constant `INADDR_ANY`
        - Configures socket to accept connections from any of the addresses of the machine

> [!info]+ A machine can have multiple network interface cards.
> ![](https://i.imgur.com/J3bUK0n.png)
>
> - Machine can be plugged into separate networks
>     - Would have a different IP on each network
>
> ![](https://i.imgur.com/QgTPtzA.png)
>
> - Machine also has an address for itself
>     - Address `127.0.0.1` called *localhost*
>         - Refers to the machine running the program

- `teach.cs.toronto.edu`
    - Teaching machine at UofT
    - Has IP address `128.100.31.200`
        - Once you start your server, could connect to it using that address
    - If you run client program on same machine:
        - Can also connect with `127.0.0.1`

##### `sin_zero`

- `sin_zero`
    - Extra padding
    - Makes `sockaddr_in` struct the same length as `sockaddr` struct
        - `len(struct sockaddr_in) == len(struct sockaddr)`

![|400](https://i.imgur.com/rapQRaY.png)

- When we `malloc` space for this struct:
    - Bytes are not reset in any way
    - Hold values that were there in memory before `malloc` call
    - % Since we are sending these bytes over the internet, we do not want to introduce any potential security problems by exposing the previous contents to potential eavesdroppers
        - ![|400](https://i.imgur.com/Pq3N0WH.png)
        - & → Use `memset` to set these 8 bytes to `0`
            - ![|400](https://i.imgur.com/JvbpKbW.png)

![](https://i.imgur.com/icLhmx1.png)

- `sockaddr` struct initialized → Ready to pass it as the second parameter to `bind`
- ! `bind` is expecting a pointer to `struct sockaddr`
    - Not `struct sockaddr_in`
    - & → Take the *address* of our struct i.e., `&addr`
    - & → *Cast* this to `struct sockkaddr *`
        - `(struct sockaddr *) &addr`
        - Indicate that we know the types do not match precisely
        - Compiler → happy

![](https://i.imgur.com/lQm8cvE.png)

- Last parameter to `bind` is **`address_len`**:
    - Length of the address that we are passing
    - Use `sizeof` on `struct sockaddr_in`

![](https://i.imgur.com/oJ14cck.png)

- % Disagreement about this historical reason for why `bind` requires this size since we already padded out `sockaddr_in` to be the same length as `sockaddr`
    - But it is needed → put it in

#### Calling `bind`

![](https://i.imgur.com/V9jWeYB.png)

- `bind` return value
    - For *error* checking
    - `0` on success
    - `-1` on failure
- & Confirming success of `bind` is important
    - Suppose port you picked is not available because it is already in use
    - `bind` will fail
    - ! Would not want program to continue running without alerting you if this happened

### `listen`

- Now we have a socket, bound to a particular port on a particular machine
- @ Need to tell the machine to start looking for connections
    - → Use `listen`

```c
int listen(int socket, int backlog);
```

- **`listen`**
    - System call
    - Tells the machine to start looking for connections
- `socket`
    - Same socket we are setting up
- `listen` return value:
    - For error checking
- ? What is `backlog`?
    - We are using a connection-oriented socket
        - → Some work involved in setting up the connection between the server and the client
        - Between the time one client starts to connect and finally gets set up, there might be *another* client that attempts to connect
        - ![|400](https://i.imgur.com/RRhStWE.png)
        - & `listen` keeps some number of these *pending connections*
            - Requests to whom it has replied trying to establish the connection but has not heard back from
    - & `listen` sets up the data structure needed to store these partial connection
    - & `backlog` is the *maximum* number that it can hold
        - ! Not the maximum number of connections to server in the end
        - Just maximum number of partially completed connections that can be held at any one time
    - Set to `5`
        - Should be plenty for our purposes

![](https://i.imgur.com/pRMdbpT.png)

```c title:server1.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <netinet/in.h>    /* Internet domain header */
#include <arpa/inet.h>     /* only needed on my mac */

int main() {
    // create socket
    int listen_soc = socket(AF_INET, SOCK_STREAM, 0);
    if (listen_soc == -1) {
        perror("server: socket");
        exit(1);
    }


    //initialize server address    
    struct sockaddr_in server;
    server.sin_family = AF_INET;
    server.sin_port = htons(54321);  
    memset(&server.sin_zero, 0, 8);
    server.sin_addr.s_addr = INADDR_ANY;

    // bind socket to an address
    if (bind(listen_soc, (struct sockaddr *) &server, sizeof(struct sockaddr_in)) == -1) {
      perror("server: bind");
      close(listen_soc);
      exit(1);
    } 


    // Set up a queue in the kernel to hold pending connections.
    if (listen(listen_soc, 5) < 0) {
        // listen failed
        perror("listen");
        exit(1);
    }
   
    struct sockaddr_in client_addr;
    unsigned int client_len = sizeof(struct sockaddr_in);
    client_addr.sin_family = AF_INET;

    int return_value = accept(listen_soc, (struct sockaddr *)&client_addr, &client_len);
    return 0;
}
```

## Setting Up a Connection

> [!goal] Explore how a server accepts connections from a client.

### `accept`

- **==`accept`==**
    - System call
    - Accepts connections from a client

```c
int accept(int sockfd, struct sockaddr *address, socklen_t *addrlen);
```

- **==`sockfd`==**
    - The listening socket that we have been setting up and configuring
- **==`address`==**
    - Type *pointer* to `struct sockaddr`
    - `accept` uses this parameter to *communicate* back the address of the client to the caller
        - The machine that is attempting to connect
    - & `address` points to a struct that holds the client’s address information
- & `accept` is a *blocking* system call
    - Waits until a connection is established
    - If you call accept, and no client is attempting to connect, accept will not return immediately → will block

```c
if (accept(listen_soc, struct sockaddr *address, socklen_t *addrlen) < 0) {
    perror("accept");
    exit(1);
}
```

#### `accept` Return Value

- % `accept` can return if there was an error
    - Need to check *return* value
    - `-1` if `accept` fails → use `perror`
- On success:
    - & `accept` returns an *integer* representing a new *socket*
        - Which we will use to communicate with the client
        - Will assign that value to a variable (so we can use it later in [[#Socket Communication]])

> [!note]+ Since the return value is being used to return a new socket, it cannot be used to give us the *address* of the client
> - Might want that information
> - Second parameter `address` holds the client’s address information

- Before calling `accept`:
    - Need to *allocate memory* for the `struct sockaddr`
- `accept` uses the pointer to access the struct and sets its fields

> [!attention]+ Recall `bind`: function’s type used a generic struct, but actual call used a specific struct for a particular address family.
> - We do the same thing here
> - Need a struct of type `struct sockaddr_in`

- Call it `client_addr`
    - Since it is going to hold the address of the machine on the *other* end of the connection

```c
struct sockaddr_in client_addr;
client_addr.sin_family = AF_INET;
```

- % Only field that we need to set is `sin_family`

```c
struct sockaddr_in client_addr;
client_addr.sin_family = AF_INET;

int return_value = accept(listen_soc, (struct sockaddr *) &client_addr, socklen_t *addrlen);
```

- Have to pass the *address* of `client_addr` and *cast* it to the required type
    - `struct sockaddr *`
    - Just like `bind`

#### `accept` Parameter: `addrlen`

- Recall:
    - `bind` had a third parameter which was the length of the address
    - Almost same thing for `accept`
    - & Except it is a *pointer* to memory that holds the address length
        - → `accept` can change the value

```c
struct sockaddr_in client_addr;
client_addr.sin_family = AF_INET;
unsigned int client_len = sizeof(struct sockaddr_in);  // New line

int return_value = accept(listen_soc, (struct sockaddr *) &client_addr, &client_len);
```

- Set the length to the size of our address
- Pass in the *address* of this value

### What Does `accept` Do?

```c title:server1.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <netinet/in.h>    /* Internet domain header */
#include <arpa/inet.h>     /* only needed on my mac */

int main() {
    // create socket
    int listen_soc = socket(AF_INET, SOCK_STREAM, 0);
    if (listen_soc == -1) {
        perror("server: socket");
        exit(1);
    }


    //initialize server address    
    struct sockaddr_in server;
    server.sin_family = AF_INET;
    server.sin_port = htons(54321);  
    memset(&server.sin_zero, 0, 8);
    server.sin_addr.s_addr = INADDR_ANY;

    // bind socket to an address
    if (bind(listen_soc, (struct sockaddr *) &server, sizeof(struct sockaddr_in)) == -1) {
      perror("server: bind");
      close(listen_soc);
      exit(1);
    } 


    // Set up a queue in the kernel to hold pending connections.
    if (listen(listen_soc, 5) < 0) {
        // listen failed
        perror("listen");
        exit(1);
    }
   
    struct sockaddr_in client_addr;
    unsigned int client_len = sizeof(struct sockaddr_in);
    client_addr.sin_family = AF_INET;

    int return_value = accept(listen_soc, (struct sockaddr *)&client_addr, &client_len);
    return 0;
}
```

- When we try to compile and run, the `accept` call blocks and waits
    - → Need to write a *client program* that will connect

### Example: Client Program

```c
int main() {
    int soc = socket(AF_INET, SOCK_STREAM, 0);
}
```

- Need to create a *stream socket* that will use *TCP* to communicate over the internet
    - Like we did in the server

#### `connect`

- **==`connect`==**
    - System call
    - Initiates a connection over a socket to the server

```c
int connect(int sockfd, const struct sockaddr *address, socklen_t addrlen);
```

- `sockfd`
    - The socket that we just created

```c
int main() {
    int soc = socket(AF_INET, SOCK_STREAM, 0);
    
    struct sockaddr_in server;
    server.sin_family = AF_INET;
    memset(&server.sin_zero, 0, 8);
    
    int connect(soc, const struct sockaddr *address, socklen_t addrlen);
}
```

- `address`
    - Address of the socket on the server to which we want to connect
    - Have to know this address in order to write our program
    - Use a struct of type `sockaddr_in`
        - Set field `sin_family` to `AF_INET`
        - Zero out the padding
        - Like we did in `bind`

<!-- break -->
- When you want to go order a latte at your local coffee shop:
    - Have to know the address in order to find the server
- & You need the machine address and the port number
    - Port number that we used for server was 54321
        - Server code had line: `server.sin_port = htons(54321);`
    - % Have to convert this to network byte order using `htons` in client code

```c
int main() {
    int soc = socket(AF_INET, SOCK_STREAM, 0);
    
    struct sockaddr_in server;
    server.sin_family = AF_INET;
    memset(&server.sin_zero, 0, 8);
    server.sin_port = htons(54321);
    
    int connect(soc, const struct sockaddr *address, socklen_t addrlen);
}
```

> [!question]+ What is the machine address?
> - What we actually need for this field is the IP address
>     - But we do not refer to machines by their IP addresses
>     - & Refer to machines by their *names*

> [!tip]+ We use `getaddrinfo` to lookup the internet address of a machine based on its name.
>
> ```c
> int getaddrinfo(char *host, char *service, struct addrinfo *hints, struct addrinfo **result);
> ```
>
> - System call
> - Powerful system call with lots of functionality to handle many variations in names and different protocols
>     - % We will show the simplest possible use case

```c
int main() {
    int soc = socket(AF_INET, SOCK_STREAM, 0);
    
    struct sockaddr_in server;
    server.sin_family = AF_INET;
    memset(&server.sin_zero, 0, 8);
    server.sin_port = htons(54321);
    server.sin_addr = ???
    
    int getaddrinfo(char *host, NULL, NULL, struct addrinfo **result)
    int connect(soc, const struct sockaddr *address, socklen_t addrlen);
}
```

- Ignore the second and third parameters setting them to `NULL`

```c
int getaddrinfo("teach.cs.toronto.edu", NULL, NULL, struct addrinfo **result);
```

- `host`
    - String which is the *name* of the host machine
- % e.g., `teach.cs.toronto.edu` is the name of the machine on which you are running my server

- `result`
    - *Address* of a *pointer* to a linked-list of structs
    - Might well be more than one address that satisfies your request for address information
    - Each element in list is:
        - Information about one of those valid addresses

> [!info]+ `getaddrinfo` expects you to make a pointer and pass the address of the pointer
> - Then it allocates the memory for the linked-list of address information
> - Sets your pointer to point to the list

```c
int main() {
    int soc = socket(AF_INET, SOCK_STREAM, 0);
    
    struct sockaddr_in server;
    server.sin_family = AF_INET;
    memset(&server.sin_zero, 0, 8);
    server.sin_port = htons(54321);
    server.sin_addr = ???
    
    struct addrinfo *result;  // New line
    
    int getaddrinfo("teach.cs.toronto.edu", NULL, NULL, &result);
    
    
    int connect(soc, const struct sockaddr *address, socklen_t addrlen);
}
```

- `getaddrinfo` system call:
    - Allocated memory for the linked list on the *heap*
    - & Provides a *function* we call to free that memory when we are finished

```c
int main() {
    int soc = socket(AF_INET, SOCK_STREAM, 0);
    
    struct sockaddr_in server;
    server.sin_family = AF_INET;
    memset(&server.sin_zero, 0, 8);
    server.sin_port = htons(54321);
    
    struct addrinfo *result;
    
    int getaddrinfo("teach.cs.toronto.edu", NULL, NULL, &result);
    
    server.sin_addr = ???
    freeaddrinfo(result);  // New line
    
    
    int connect(soc, const struct sockaddr *address, socklen_t addrlen);
}
```

- Before we free the memory:
    - & Need to use the *address information* to set the server address
- % We will only look at the first address information struct from linked list
    - `result->ai_addr`
        - Holds the information we need
        - ! Type `sockaddr`
            - Generic address type (that we have seen before)
        - → Cast `result->ai_addr` to `sockaddr_in *` — the type for internet addresses

```c
    server.sin_addr = (struct sockaddr_in *) result->ai_addr  // Not done yet!
    freeaddrinfo(result);
```

- From that `sockaddr_in` struct:
    - & Can look at only the `sin_addr` field
    - Assign that to to the `sin_addr` field of the struct we are setting up for our connect call (e.g., `server`)

```c
int main() {
    int soc = socket(AF_INET, SOCK_STREAM, 0);
    
    struct sockaddr_in server;
    server.sin_family = AF_INET;
    memset(&server.sin_zero, 0, 8);
    server.sin_port = htons(54321);
    
    struct addrinfo *result;
    
    int getaddrinfo("teach.cs.toronto.edu", NULL, NULL, &result);
    
    server.sin_addr = ((struct sockaddr_in *) result -> ai_addr)->sin_addr;
    freeaddrinfo(result);
    
    
    int connect(soc, (struct sockaddr) &server, sizeof(struct sockaddr_in));  // New
}
```

- We now have a `struct sockaddr_in` that holds the server information
- Can pass it into `connect` as second parameter
    - But need to *cast* to `struct sockaddr`
        - (from type `struct sockaddr_in`)
- Also provide its *length*
    - Third argument

```c title:client1.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <netinet/in.h>    /* Internet domain header */
#include <netdb.h>
#include <sys/socket.h>
#include <arpa/inet.h>

int main() {
    // create socket
    int soc = socket(AF_INET, SOCK_STREAM, 0);
    if (soc == -1) {
        perror("client: socket");
        exit(1);
    }

    //initialize server address    
    struct sockaddr_in server;
    server.sin_family = AF_INET;
    server.sin_port = htons(54321);  
    memset(&server.sin_zero, 0, 8);
    
    struct addrinfo *ai;
    char * hostname = "teach.cs.toronto.edu";

    /* this call declares memory and populates ailist */
    getaddrinfo(hostname, NULL, NULL, &ai);
    /* we only make use of the first element in the list */
    server.sin_addr = ((struct sockaddr_in *) ai->ai_addr)->sin_addr;

    // free the memory that was allocated by getaddrinfo for this list
    freeaddrinfo(ai);

    int ret = connect(soc, (struct sockaddr *)&server, sizeof(struct sockaddr_in));

    printf("Connect returned %d\n", ret);
    return 0;
}
```

> [!warning]+ In final code, you should use `connect` return value to confirm that connection worked.

- Server code no longer blocks on the `accept` call
    - It accepted the connection
    - Since that accept was the last line in the server program, it also finished running

### What Happens if We Run the Client Code without a Server Running?

- `connect` returns a `-1`
    - Connect was unsuccessful
    - & Nothing waiting at that port

### What Happens if We Run the Client Code and Stop the Server before the `listen` Call?

- `socket` and `bind` have run
    - There is a socket at this address
    - But server is not listening yet for connections
- `connect` returns `-1`
    - & Even though server is running, it has not yet set up the queue to listen for connections

## Socket Communication

> [!goal] Demonstrate how to use that connection to communicate between the two programs

### Summary so far

![](https://i.imgur.com/8bxW2z5.png)

`server.c`

- `socket` system call
    - Used to *create* a socket on which the server will listen for connections
- `bind`
    - Bind — or assign — that socket to a particular port and the address of the machine on which we are running the server
- `listen`
    - Tell the socket to start listening for partial connections
- `accept`
    - *Blocking* call
    - Returns only if there is an error or when connection is made

### Writing from Server, Reading from Client

> [!important]+ When `accept` returns, it returns the descriptor for a new socket
> - We use this socket to *communicate* with the client

![](https://i.imgur.com/brmvcf6.png)

- % Changed the name of this variable to `client_socket` to better represent this
- ? What happens to our initial listening socket `listen_soc`?
    - Still there
    - Could call `accept` on it again
        - But `accept` will block until there is a second client connecting
        - While your server is blocked, it cannot be interacting with the first client!
        - % Need to combine this with `fork` or `select` to be able to handle multiple clients simultaneously

- $ socket is type `int`
- Recall:
    - `socket` returns an index in the file-descriptor table
    - % Sometimes refer to the socket as the **socket descriptor**
- Once we have the stream socket set up between the server and client:
    - & Use this socket descriptor just like a file descriptor
        - Use `read` and `write` system calls

![](https://i.imgur.com/RU3Nf8A.png)

Consider making the server write to the client first.

- Send 7 characters
    - `\r` and `\n` are each one character
- We do not explicitly send the null character at the end in this example

- **Network newline**
    - `\r\n`
    - We needed to specify a specific order for the bytes in the multi-byte numeric types
        - → Machines that use different byte orders can work together
    - Have to specify how to identify a newline
        - → Machines that use different sequences of characters will render strings appropriately

![](https://i.imgur.com/e8lrcLb.png)

- ! `printf` expects `buf` to be a string → needs a null terminator
    - We did not write a null terminator to the socket
    - Need to explicitly add `\0` after the bytes that we read into `buf`

![](https://i.imgur.com/br7X0R8.png)

- Compile both programs
    - Server code first
        - Blocks; waiting for a connection
    - Run client → Make a connection
- $ `printf` statement printed the newline as well
    - Since we sent that

### Writing back to the Server

![](https://i.imgur.com/ovnmdUm.png)

- Use the same socket
- Specify the exact number of bytes to write in `write`
- Declare enough memory for 10 characters
    - Called it `line` this time to demonstrate that there is nothing special about the name `buf`

<!-- break -->
- This time:
    - Reading 10 characters
    - Using all the memory available
- When you null-terminate the string:
    - Do not have room to add to the values that were read
    - Have to replace the last one

![](https://i.imgur.com/jYACm93.jpeg)

- Server reads 10 characters
- Prints 9 of them
    - Since null terminator

![](https://i.imgur.com/S2Y6Dol.png)

- Also a good idea to `close` sockets

```c title:server2.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <netinet/in.h>    /* Internet domain header */
#include <arpa/inet.h>     /* only needed on my mac */

int main() {
    // create socket
    int listen_soc = socket(AF_INET, SOCK_STREAM, 0);
    if (listen_soc == -1) {
        perror("server: socket");
        exit(1);
    }


    //initialize server address    
    struct sockaddr_in server;
    server.sin_family = AF_INET;
    server.sin_port = htons(54321);  
    memset(&server.sin_zero, 0, 8);
    server.sin_addr.s_addr = INADDR_ANY;

    // bind socket to an address
    if (bind(listen_soc, (struct sockaddr *) &server, sizeof(struct sockaddr_in)) == -1) {
      perror("server: bind");
      close(listen_soc);
      exit(1);
    } 


    // Set up a queue in the kernel to hold pending connections.
    if (listen(listen_soc, 5) < 0) {
        // listen failed
        perror("listen");
        exit(1);
    }
   
    struct sockaddr_in client_addr;
    unsigned int client_len = sizeof(struct sockaddr_in);
    client_addr.sin_family = AF_INET;

    int client_socket = accept(listen_soc, (struct sockaddr *)&client_addr, &client_len);
    if (client_socket == -1) {
        perror("accept");
        return -1;
    } 


    write(client_socket, "hello\r\n", 7);

    char line[10];
    read(client_socket, line, 10);
    /* before we can use line in a printf statement, ensure it is a string */
    line[9] = '\0';
    printf("I read %s\n", line);

    return 0;
}
```

```c title:client2.c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <netinet/in.h>    /* Internet domain header */
#include <netdb.h>
#include <sys/socket.h>
#include <arpa/inet.h>

int main() {
    // create socket
    int soc = socket(AF_INET, SOCK_STREAM, 0);
    if (soc == -1) {
        perror("client: socket");
        exit(1);
    }


    //initialize server address    
    struct sockaddr_in server;
    server.sin_family = AF_INET;
    server.sin_port = htons(54321);  
    memset(&server.sin_zero, 0, 8);
    
    struct addrinfo *ai;
    char * hostname = "teach.cs.toronto.edu";

    /* this call declares memory and populates ailist */
    getaddrinfo(hostname, NULL, NULL, &ai);
    server.sin_addr = ((struct sockaddr_in *) ai->ai_addr)->sin_addr;


    // free the memory that was allocated by getaddrinfo for this list
    freeaddrinfo(ai);

    int ret = connect(soc, (struct sockaddr *)&server, sizeof(struct sockaddr_in));
    if (ret == -1) {
        perror("client: connect");
        exit(1);
    }

    printf("Connect returned %d\n", ret);

    char buf[10];
    read(soc, buf, 7);
    buf[7] = '\0';
    printf("I read %s\n", buf);

    write(soc, "0123456789", 10);
    return 0;
}
```
