# Chapter  1 - The basics

## Introduction

- Application consisting of **two or more parts** , each one running on some **seperate computing device** and communicates with parts over a network is a **==distributed application==** 
-  Two devices must agree on a **==communication protocol==**

- **TCP/IP** is the **standard** , combining into a stack of many other protocols.
- Distributed software developers often deal with **TCP** or **UDP** 
- **Lower** **layer** protocols are often hidden from the developer and **handled** by the **operating** **system** and network devices.

---

> TCP
>
> 1. reliable, consistent order
> 2. connection establishment
> 3. point to point no multicast
> 4. stream oriented, stream of bytes coming in.

> UDP
>
> 1. unreliable, not ensured to be error free
> 2. connectionless
> 3. one to one and one to many communication models
> 4. datagram oriented

- Use TCP over internet for reliability.

---

- **==Berkely sockets api==** $\to$ introduce idea of *sockets*.

- All OS have this api with some variations

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220219170429306.png)

- Generic API thus complex.
- C style functional API with poor type system and thus error prone.

- Boost API built around the idea of a *socket*.
  - This wraps the raw sockets api , providing a OO interface to it
    - Intended to **simplify network programming** in *several* ways

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220219175952948.png)

## Creating an Endpoint

- Require **IP address** of the *host* on which the **server application** is *running* and the **protocol** *port number associated*
- Pair of *uniquely identifying* values is an **==endpoint==** 

---

- IP $\to$ represented as **string** containing some *address*
  - HEX = IPV6
  - string = IPv4
- Server IP address can be **provided** to the **client application** in some *indirect form* as a string containing a DNS name such as **localhost**
- Alternatively an **integer value**

---

- Must resolve DNS

---

- Endpoint $\to$ specify to OS unique configuration.
- Host running $\to$ single network interface and single IP address assigned, server must only listen.
- Host may have **more than one network interface** thus *multiple IP addresses*.
- Sometimes hard to determine which IP the messages sent by clients must be delivered to the **host**

---

- Server should listen **on all IP addresses** available on the host
- This ensures the **server receives all messages** arriving at any **IP address** and **particular protocol** part.

---

- Endpoint two goals

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220219182348354.png)

### Server / Client Endpoints

```c++
/* client* / 

#include <iostream>
#include <boost/asio.hpp>

int main() {
	
	/* ip address and port number  */

	std::string rawIPAddress = "127.0.0.1";
	unsigned short portNum = 3333;

	/* store information on error whilst parsing the ip address*/

	boost::system::error_code ec;

	/* Use IP protocol version independent (agnostic) address representation */

	boost::asio::ip::address ipAddress = boost::asio::ip::address::from_string(rawIPAddress);

	if (ec.value() != 0) {
	
		/* provided IP address is invalid, breaking execution */
		std::cout << "failed to parse IP address. Error code = "
				  << ec.value()  << ". Message: " << ec.message();
		return ec.value();
	}

	/* create an endpoint*/

	boost::asio::ip::tcp::endpoint ep(ipAddress, portNum);

	/* 
	    Endpoint ready and can be used to specify a particular server in the network
		that the client wishes the communicate with.
	*/


}
```

```c++
/*server*/ #include <iostream>
#include <boost/asio.hpp>

int main() {

	/* assume that the server application has obtained the protocol port number */

	unsigned short portNum = 3333;

	/* create special object to specify all IP addresses available on the host 
		here we assume the server works over IPV6 protocol
	*/

	boost::asio::ip::address ipAddress = boost::asio::ip::address_v6::any();

	/* create endpoint */

	boost::asio::ip::tcp::endpoint ep(ipAddress, portNum);

	/* endpoint created, can now specify IP addresses and a port number on which the server applications
		wish to listen for incoming connections.
	*/

}

```



### Code Explanation Key Points

- Boost has a type system for each IP address:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220219195828734.png)

- Overload used in`from_string` to create the addresses.

```c++
static asio::ip::address from_string(
    const std::string & str,
    boost::system::error_code & ec);
```

- This checks for validity of the ip address format.
- Server only provided with a **port number** as we wish to listen to **all ip addresses** being a server of course.
- use `any()` to accept any address incoming, in our case for ipv6 addresses.
- Create endpoint in both samples.

---

- We used `endpoint` class declared in the scope `asio::ip::tcp` class
  - Viewing the declaration:

```c++
class tcp
{
public:
  /// The type of a TCP endpoint.
  typedef basic_endpoint<tcp> endpoint;

  //...
}
```

- Thus this endpoint **specializes** a generic endpoint, in our case for **TCP**.
  - UDP thus very similar.

```c++
class tcp
{
public:
  /// The type of a TCP endpoint.
  typedef basic_endpoint<tcp> endpoint;

  //...
}
```

- So just change `asio::ip::tcp::endpoint` to `asio::ip::udp::endpoint` to alter the protocol used.

## Creating an active socket

- TCP/IP tells us **nothing about sockets** 
- Also tells us **nothing** how to *implement* the TCP/UDP protocol software API through which has **the software functionality** can be **consumed by the application**.

---

- We do most of the implementation as TCP protocol provides only **functional requirements**.
- Berkely sockets api implements this.

---

- **==passive socket==** $\to$ passively waits for incoming requests from **remote applications**
  - These do not **not take part in user data transformation**
- **==active socket==** $\to$ send and receive data / initiate a connection establishment process.

```c++
#include <iostream>
#include <boost/asio.hpp>

int main() {
	
	/* step 1 - instance of io_service class required by socket constructor */
	/*https://stackoverflow.com/questions/60997939/what-exacty-is-io-context */
	boost::asio::io_context context;

	/* step 2 - creating an object of tcp class representing a tcp protocol with IPV4 as the underlying protocl */

	boost::asio::ip::tcp protocol = boost::asio::ip::tcp::v4();

	/* step 3 - instantiating an active TCP socket object */

	boost::asio::ip::tcp::socket sock(context);

	/* used to store information on errors while opening a socket*/

	boost::system::error_code ec;

	/* opening socket*/

	sock.open(protocol, ec);

	if (ec.value() != 0) {
		/* failure to open a socket */
		std::cout << 
			"Failed to open the socket! Error code = "
			<< ec.value() 
			<< ". Message: " << ec.message();
		return ec.value();
	}

	return 0;

}
```

### Code Important notes

- `asio::io_service` / `asio::io_context` $\to$ central component to **Boost.Asio** 
  - Gives access to the **network I/O** services of the **underlying OS**.
- Asio sockets obtain access to these by **this class** therefore all **socket constructors** require an object of `asio::io_service` 

---

- Create instance of `asio::ip::tcp` to represent TCP protocol.
- Acts like a **data structure** containing values relevant to the **protocol**

---

- `asio::ip::tcp` has **no public constructor** 
  - Provides **two static methods** to constructor.
    1. `asio::ip::tcp::v4()` 
    2. `asio::ip::tcp::v6()`
- Contains other useful items
  - `asio::tcp::endpoint` 
  - `asio::tcp::socket` 
  - `asio::tcp::acceptor` 

```c++
namespace boost {
namespace asio {
namespace ip {

  // ...
  class tcp
  {
  public:
    /// The type of a TCP endpoint.
    typedef basic_endpoint<tcp> endpoint;
    
    // ...
  
    /// The TCP socket type.
    typedef basic_stream_socket<tcp> socket;

    /// The TCP acceptor type.
    typedef basic_socket_acceptor<tcp> acceptor;
    
    // ...
```

## Creating a passive socket

- Passive sockets only used in **server applications** or **hybrid applications** that *may play both roles*  of **client / server**
- Passive sockets are defined for **TCP** as **UDP** does **not imply connection establishment**, there is **no need for a passive socket** when the **communication** is performed **over UDP**.

## Resolving a DNS name

```c++
#include <iostream>
#include <boost/asio.hpp>
/*

	Raw IP addresses are inconvenient for humans to use thus use DNS.

	DNS is an alias for one or more IP addresses but not the devices.

	DNS adds level of indirection in addressing some server.

	DNS acts as a distributed database to store mappings of DNS names to corresponding IP address

	DNS resolution is the process of locating the IP from alias.

*/


int main() {
	
	/*
		step 1 - Assume client application obtained DNS name and protocol port number, representing as strings

		> Obtain DNS name and protocol part, represent as strings, often provided by application UI.
	*/

	std::string host = "samplehost.com";
	std::string port_num = "3333";

	/*
		step 2
		> used by the resolver to access underlying OS services during DNS resolution process.
	*/

	boost::asio::io_service ios;

	/*
		step 3 create a query

		> create object boost::asio::ip::tcp::resolver::query, represents query to a DNS.
		> Contains DNS name to resolve / port number to construct endpoint after DNS resolution complete
		> Contains also a set of flags controlling some specific aspect of resolution process, represented as bitmap.
		> Pass these to query class constructor
		> Service specified as port number for protool not as HTTP/FTP service we use numeric_service flag, to parse our port properly.

	*/

	boost::asio::ip::tcp::resolver::query resolver_query(host, port_num, boost::asio::ip::tcp::resolver::query::numeric_service);

	/*
		 step 4 creating a resolver

		 > provides DNS name resolution functionality
		 > to perform resolution , requires services of underyling OS and obtains via io_service passed as constructor arg.
	*/

	boost::asio::ip::tcp::resolver resolver(ios);

	/*
		store information about errar that occurs
	*/

	boost::system::error_code ec;

	/*
		step 5 

		> Perform DNS resolution. 
		> error code passed in for any failures.

		> if succesful returns iterator to first element of collection showing resolution results
			> of type boost::asio::tip::basic_resolver_entry<tcp> class.
		> There are many objects in collection as the total number of IP addresses that resolution yielded
			> Each collection element stores boost::asio::ip::tcp::endpoint classed instantiated from one IP address resulting 
			  from the resolution process along with port number provided with query object.
	
		> endpoint object accessed via boost::asio::ip::basic_resolver_entry<tcp>::endpoint() method.
	*/
	boost::asio::ip::tcp::resolver::iterator it = resolver.resolve(resolver_query, ec);

	boost::asio::ip::tcp::resolver::iterator it_end;

	for (; it != it_end; ++it) {
		/* 
			Here we can access the endpoint like this 
			
			often multiple endpoints thus request responses from multiple until desired output.
		*/
		boost::asio::ip::tcp::endpoint ep = it->endpoint(); /* (*it).endpoint()*/
	}

	/*
		handling any potential errors
	*/

	if (ec.value() != 0) {
		/* failed to resolve DNS name, breaks execution*/
		std::cout << "failed to resolve DNS name" << "Error code = " << ec.value() << " . Message = " << ec.message();

		return ec.value();
	}

	return 0;

}
```

## Binding a socket to endpoint

```c++
#include <iostream>
#include <boost/asio.hpp>

/*
	
	> before active socket communicates with remote applications
	> before passive socket accepts incoming request.
	
	- must be associated with some particular local ip -

	> associating a socket with an endpoint is called binding

	> when socket binds to endpoint
		> all network packets coming into the host from the network with that endpoint as the target address
		  will be redirected to this socket by the operating system.

	> all data coming out from a socket bound to a particular endpoint will be output from the host to
		network through a network interface associated with corresponding IP address in the endpoint.

	-----------------------------------------------------------

	> Operations may bind unbound sockets implicitly.
		> bind implictly to an IP address and protocol port number chosen by OS.

	> client often doest not explicitly state an active endpoint

	------------------------------------------------------------

	> when socket binding is delegated to the OS, there is no guarantee that it will be 
	  bound to the same endpoint each time
	  > Even if a single network interface and a single IP address on the host, socket may be bound to
		some different protocol port number everey time the implict binding is performed.

	> client applications dont care which IP and protocol port number its active socket
		will be communicating with the remote applicaition
	> server application often binds its acceptor socket to a particular endpoint explictly.
	 > this is as the server must be known to all clients to communicate with it and should stay the same after the server application
	   is restarted.


*/

int main() {
	
	/* step 1 assume server application has obtained protocol part number */

	unsigned short portNum = 3333;

	/*
      step 2 creating an endpoint

	  > represents any ip address ipv4 on the system and the specified

	*/

	boost::asio::ip::tcp::endpoint ep(boost::asio::ip::address_v4::any(), portNum);

	/*
		used by acceptor class constructor
	*/

	boost::asio::io_service ios;

	/* 
		step 3 - creating and opening an acceptor socket

		> endpoint in step 2 contains infomration about transport protocol and verison of underlying IP protocol ipv4

		> dont create another object representing the protocol , just call.protocol()
	*/

	boost::asio::ip::tcp::acceptor acceptor(ios, ep.protocol());


	/* store information on error whilst parsing the ip address*/
	boost::system::error_code ec;

	/*
		step 4 - binding acceptor socket

		> passing object endpoint and error code , to say our acceptor should bind to this.

		> if succeeeds , the acceptor socket is bound to the corresponding endpoint and is ready to start listening
		  for incoming connection requests on that endpoint
	*/

	acceptor.bind(ep, ec);


	if (ec.value() != 0) {
	
		/* provided IP address is invalid, breaking execution */
		std::cout << "failed to bind then acceptor socket = "
				  << ec.value()  << ". Message: " << ec.message();
		return ec.value();
	}

	return 0;

}
```

## Connecting to a socket

```c++
#include <iostream>
#include <boost/asio.hpp>
/*
	creating socket

	client > create active socket > call .connect(server target) > 

	server > receives request > creates active socket on its side marking it as connected to a specific client
			> replies to the client with message acknowledging that connection is successfully set up on the server side.

	client > marks it socket as connected to server, sends final message to acknowledge the connection successful on client side.

	server receives this creating the logical connection and establishes.

	assumes socket A > connected to socket B both only communicate with one another

	adding another socket means to break the connection 
*/
int main() {
	
	/* ip address and port number, assumed obtained by UI  */

	std::string rawIPAddress = "127.0.0.1";
	unsigned short portNum = 3333;

	try {
		/*
			step 2 - creating an endpoint designating a target server application
		*/

		boost::asio::ip::tcp::endpoint ep(boost::asio::ip::address::from_string(rawIPAddress), portNum);

		boost::asio::io_service ios;

		/*
			step 3 - create an open socket, active socket instantiated and opened.
		*/
		boost::asio::ip::tcp::socket sock(ios, ep.protocol());

		/*
		*  step 4 - Connecting a socket
		* 
		*	pass our specified endpoint, denoted by ip address and port
		*	this is performed synchronously, which means the method blocks the caller thread until either connection
		*	operation is established or an error occurs.
		* 
		*	> performs bind for us.
		* 
		*/

		sock.connect(ep);

		/*
			sock now connected to the server applicaiton and can be used to send data to or receive data from it
		*/

	}

	/*
		connect() throws exception of boost::system::system_error if operation fails

		so does the overload of boost::asio::ip::address::from_string() static method in step 2.

		thus both calls enclosed in a try catch block.

		both methods have overloads that dont throw exceptions and accept an object of the boost::system::error_code class

		which used to conduct error information in the caller in case the operation fails , for this case, using exceptions
		to handle errors makes the code better structured.
	*/
	catch (boost::system::system_error& e) {
		std::cout << " Error code = "
			<< e.code() << ". Message: " << e.what();
		return e.code().value();
	}

}
```

### DNS version

```c++
#include <iostream>
#include <boost/asio.hpp>

int main() {

	std::string host = "fart.com";
	std::string portNum = "3333";

	boost::asio::ip::tcp::resolver::query resolver_query(host, portNum, boost::asio::ip::tcp::resolver::query::numeric_service);

	boost::asio::io_context ios;
	boost::asio::ip::tcp::resolver resolver(ios);

	try {
		
		boost::asio::ip::tcp::resolver::iterator it = resolver.resolve(resolver_query);

		boost::asio::ip::tcp::socket sock(ios);


		/*
			iterates over each endpoint from dns query and connects to one of them

			throws exception if it fails to connect or any other errors.
		*/
		boost::asio::connect(sock, it);


		/*
			socket 'sock' connected to server application can be used to send data to or receieve data from it
		*/

	}


	catch (boost::system::system_error& e) {
		std::cout << " Error code = "
			<< e.code() << ". Message: " << e.what();
		return e.code().value();
	}

	return 0;

}
```

## Accepting connections

```c++
#include <iostream>
#include <boost/asio.hpp>

/*
	----------------- Client Side ----------------------
	client connect to server over TPC must establish logical connection

	client allocates an active socket 
		> .connect() > connection establishment request sent to server

	----------------- Server side -----------------------

	Server must configure to accept request otherwise all connections rejected by OS

	1. creates and opens an acceptor socket and binds to its endpoint
	2. Switch acceptor to listening mode to accept new connections
		> enables a queue for pending requests associated with accceptor socket to start accepting requests
	3. New connection request arrives 
		> received by OS initially > place in queue.
		> When in queue > available to server for processing > dequeues when ready
	
	- Acceptor socket only establishes connections and no further processing.

	4. When processing a pending connection
		> acceptor socket allocates a new active socket
		> binds to endpoint chosen by OS
		> connects to corresponding client applicaiton 

*/

int main() {
	
	/*
		size of queue containing pending connnection request.
	*/
	const int BACKLOG_SIZE = 30;

	unsigned short port_num = 3333;

	boost::asio::ip::tcp::endpoint ep(boost::asio::ip::address_v4::any(), port_num);

	boost::asio::io_context ios;

	try {
		
		boost::asio::ip::tcp::acceptor acceptor(ios, ep.protocol());

		acceptor.bind(ep);


		/*
			call acceptor listen pass our back log size constant as argumet

			this switches the acceptor socket into the state in which it listens to incoming requests 

			all request are rejected if this acceptor is not listening.

			BACKLOG_SIZE specifies size of queue maintained by the operating system to which it puts connection requests
			arriving from the client

			request stays in the queue and are waiting for the server application to dequeue and process them

			when this queue becomes full, new connection request are rejected by the operating system
		*/

		acceptor.listen(BACKLOG_SIZE);

		/*
			accept active socket as an arguemnt

			checks if queue associated with socket contains pending connections
				> empty > method blocks execution until new connection request arrives to an endpoint
				to which the acceptor socket is bound and OS places in the queue.
		*/
		boost::asio::ip::tcp::socket sock(ios);


		/*
			active socket passed to accept() method as an arg is connected to corresponding client applicaiton
			which issue this connection request.

			> this scans the acceptor queue and connects the socket located in the newest queue to our servers
			socket here which creates a established socket to socket client - server communication.


		*/
		acceptor.accept(sock);

		/*
			'sock' now connected to the client application and can be used to send data to or receive data from it.
		*/

	}
	catch (boost::system::system_error& e) {
		std::cout << "Error occured! Error code = " << e.code()
			<< ". Message: " << e.what();
		return e.code().value();
	}

	return 0;
}
```

