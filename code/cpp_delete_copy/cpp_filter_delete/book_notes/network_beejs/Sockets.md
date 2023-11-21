# Sockets

- Sockets are a method to communicate using **unix file descriptors**.
- Programs communicate file **file descriptors** $\to$ some integer associated with an **open file**.
- This file may be
  1. Network connection
  2. FIFO
  3. pipe
  4. terminal
  5. on disk file
  6. ...
- call `socket()` to obtain this **file descriptor**.

## Types of sockets

- ==stream sockets== $\to$ `SOCK_STREAM`, ordered socket connection and error free.
- ==datagram sockets== $\to$ `SOCK_DGRAM` aka *connectionless sockets* but can call `connect()` if required.

---

- Telnet uses **streams sockets** $\to$ telnet on port 80 of some site with a `GET / HTTP/1.0`  request it gives us **html**.
- HTTP uses **stream sockets**
- Telnet to some website on **port 80** $\to$ type `"GET / HTTP/1.0"` then return **twice** to dump html back.

---

- Stream sockets $\to$ use the **transmission control protocol** $\to$ $TCP$. 
- TCP ensures data arrives **sequentially** and **with no errors**.
- IP deals with **internet routing** and **data integrity**.

---

- Datagram sockets $\to$ unreliable / connectionless
- Sending a **datagram** means it **may arrive** or **may arrive out of order**
- The data send however is still **error free** if it arrives.
- Uses the **UDP protocol** (*user datagram protocol*) as opposed to the **TCP protocol** to send its packets 

---

- UDP does not **require an open connection** as with **stream sockets**.
- Send packet with **IP header** and send without expecting the other end to receive every packet.
- Used when **TCP stacks** are *not available* or a few packets being dropped is **okay** $\to$ *tftp* / *dhcpcd* / *multiplayer games* / *streaming audio* / *video conferencing*.

---

- tftp expects an **ACK packet** on its **UDP packet** to confirm a packet has been acknowedlged and the **file has been received**.
- No reply after **set time** $\to$ resends the **packet**.

---

- Much **faster** to use **UDP** and *fire and forget* the packets.
- UDP useful for **state spams** such as *locations* of things in a multiplayer game.

## Low level nonsense and network theory

- How the `SOCK_DGRAM` packets are **built**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220731183959321.png)

- Packet wrapped ina  **header** by the **first protocol**, and then each **protocol** further wraps up the **layer below it**.

---

- Hardware receives a packet $\to$ **hardware strips** the **ethernet header**.
- The kernel strips the **IP / UDP headers**
- The TFTP program strips the **TFTP** header and finally obtains the **data**.

---

- There is the **==ISO/OSI==** model
- Ensures programs can be made the same way in terms of **data communicating** in the *levels below*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220731185053250.png)

- Physical is just the **serial / ethernet**
- Application is closest to the end user where they **interact** with the **network**.
- A layered model more consistent to **UNIX** would be:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220731185158089.png)

- Lots of layers but all we must do is `send()` the data in **stream sockets**
- For **datagram sockets** $\to$ we select `sendto()` for the encapsulation.
- Kernel constructs the **transport layer** and **internet layer** for us and **hardware** does the **network access layer**.

# IP Addresses, `structs` and Data munging

## IP Addresses, versions 4 and 6

- **IPV4** $\to$ madef of **4 bytes** (4 octets) and was written in **dots and numbers form** like $192.0.2.111$. 
- Potentially going to **run out of these addresses** as they are *limited* which was not predicted thus now have **IPV6**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220731190607593.png)

- There are **16 octets** in total, each colon seperates **2 bytes** thus **128 bits** in total which exponentially increases the **max number of addresses** to a level too high to even comprehend
- You can compresse address that contains **0000** between colons

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220731190908824.png)

- `::1` is the **loopback address** $\to$ means the **machine currently being run on**
- IVP4 loopback address $\to$ `127.0.0.1`.

---

- Can represent `IPV4` in terms of `IPV6`.
- `192.0.2.333` $\to$ `"::ffff:192.0.2.33".` 

---

### Subnets

- In **organizational reasons** $\to$ declare a section of IP address to be the **network portion** and the **remainder** the **host portion**

- `ipv4` $\to$ `192.0.2.12` , say the **first three bytes** are the *network* and the **last byte** was the *host*.
- For this example $\to$ talking about **host 12** on `192.0.2.0`. 

---

- Class A $\to$ more host bytes 
- Class C $\to$ more network bytes, one byte of hosts.

---

- Network portion $\to$ the **==netmask==** $\to$ you *bitwise AND* with the **IP address** to get the **network number out of it**.
- Often the netmask is `255.255.255.0` 
- Masking `192.0.2.12` obtains `192.0.2.0` for example.

---

- Was not enough therefore $\to$ ran out of Class A and C eventually.
- Remedy by making the **powers** allowed for the **netmask** to be an **arbitrary** number of bits, not just 8,16,24.
- Possible netmask $\to$ `255.255.255.252`, this is **30 bits** of **networks** and **2 bits of host** enabling *four hosts* on the **network**. (252 = 0b1111100 = 252, 00 gives 4 hosts available after the 1 masks with everything)
- netmasks are **1 bits** follwed by **0 bits**.

---

- Was not very intuitive therefore use a **slash** after the **IP address** followed by the **number of network bits**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220731201031112.png)

### Port numbers

- Layered network model $\to$ IP split from Host to Host **transpory layer** (TCP / UDP)

---

- TCP / UDP require **port numbers** alongside the **IP address** which is some **16 bit number** acting as the **local address** for some connection.
- Different services running under the **same IP address** use a **different port number**.
- Ports under 1024 are often important such
  1.  telnet - 23
  2. HTTP - 80
  3. SMTP - 25

## Byte Order

- The order in which **bytes are stored** such as **two byte hex** `b34f` $\to$ store each byte **sequentially** $\to$ called **==Big Endian==**.
- Other order is the **network byte order** as network types operate using this.
- Computer stores numbers in **host byte order**
- Intel `80x86` $\to$ host byte order is **little endian**
- Motoroal `68k` $\to$ host byte order is **big endian**  

---

- When building packets / filling out **data structures** $\to$ make sure your *two / four byte* numbesr are in the the **network byte order**.
- Assume the **host byte order** is *incorrect* and run the value through some function to set it to **network byte order**.
- Function does the **conversion** for us, making the code portable to machines of **varying endianness**.

---

- Two numbers to convert
  1. short, 2 bytes
  2. long, 4 bytes
- The functions work for **unsigned variants also**.
- Convert a *short* from **host byte order** to **network byte order**.
- Start with `"h"` for `"host"`  $\to$ follow it with `"to"` then `"n"` for network and `"s"` for short $\to$ `htons()` (*host to network short*)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220731203136071.png)

- Always convert **numbers** to **network byte order** before they go out on the wire then convert to **host byte order** when coming off.
- No 64 bit variant and for **floating point** $\to$  check out **serialization**.

## Structs

- **==Socket descriptor==** $\to$ `int` 

- `addrinfo` $\to$ prep socket **addres structures** for subsequent use.
  - used in:
    1. Host / service name lookup
- First thing called when **making a connection**.

```c
            struct addrinfo {
                int              ai_flags;     // AI_PASSIVE, AI_CANONNAME, etc.
                int              ai_family;    // AF_INET, AF_INET6, AF_UNSPEC
                int              ai_socktype;  // SOCK_STREAM, SOCK_DGRAM
                int              ai_protocol;  // use 0 for "any"
                size_t           ai_addrlen;   // size of ai_addr in bytes
                struct sockaddr *ai_addr;      // struct sockaddr_in or _in6
                char            *ai_canonname; // full canonical hostname

                struct addrinfo *ai_next;      // linked list, next node
            };
```

- Load this struct up $\to$ call `getaddrinfo()` $\to$ return **pointer** to a **new linked list** of *these structures* which is *filled* with the **vital data**.

---

- `ai_family` $\to$ use to set `ipv6` or `ipv4`. Alternatively use `AF_UNSPEC` to use **any** $\to$ ensures the **code is** ip-agnostic.
- This is a **linked list** $\to$ `ai_next` points at the **next element** $\to$ can be **several results** to choose from.

---

- `ai_addr` $\to$ field in the `addrinfo` is a *pointer* to `struct sockaddr`.
- This is where **we start** discussing internals of **IP address structure**.

---

- Call `getaddrinfo()` to fill out the `struct addrinfo` for you sometimes.
- Must peer inside the strcuts to get the **values out**.

---

- Some `struct sockaddr` holds socket address information for many **types of sockets**

```c
struct sockaddr {
  unsigned short    sa_family;    // address family, AF_xxx
  char              sa_data[14];  // 14 bytes of protocol address
}; 
```

- `sa_family` is a variety of things , often `AF_INET` for ipv4 and `AF_INET6` for ipv6.
- `sa_data` $\to$ contains **destination address** and **port number** for the socket.

---

- To deal with `struct sockaddr` $\to$ programmers create `struct sockaddr_in` (*internet*) to be used with `ipv4`.

- A `struct sockaddr_in` may be **cast** to `struct sockaddr` , vice versa.
- Even though `connect()` wants a `struct sockaddr*` $\to$ may use `struct sockaddr_in` and cast at **the last minute**.

```c
// (IPv4 only--see struct sockaddr_in6 for IPv6)
    
    struct sockaddr_in {
        short int          sin_family;  // Address family, AF_INET
        unsigned short int sin_port;    // Port number
        struct in_addr     sin_addr;    // Internet address
        unsigned char      sin_zero[8]; // Same size as struct sockaddr
    };
```

- `sin_zero` (included to **pad** the structure to the **length** of a `struct sockaddr`) set to **all zeos** using `memset()`.

- `sin_family` is same as `sa_family` in `struct sockaddr` and set as `AF_INET`. 
- The `sin_port` must be **in network byte order** using `htons()`. 

---

- See `sin_addr` field is a `struct in_addr`. 

```c
// (IPv4 only--see struct in6_addr for IPv6)
    
// Internet address (a structure for historical reasons)
struct in_addr {
    uint32_t s_addr; // that's a 32-bit int (4 bytes)
};
```

- Declare `ina` to be of type `struct sockaddr_in` $\to$ `ina.sin.add.s_addr` references the **4 byte IP address** (NBO)

---

- IPV6 version

```c

    struct sockaddr_in6 {
        u_int16_t       sin6_family;   // address family, AF_INET6
        u_int16_t       sin6_port;     // port number, Network Byte Order
        u_int32_t       sin6_flowinfo; // IPv6 flow information
        struct in6_addr sin6_addr;     // IPv6 address
        u_int32_t       sin6_scope_id; // Scope ID
    };
    
    struct in6_addr {
        unsigned char   s6_addr[16];   // IPv6 address
    };
```

- 
