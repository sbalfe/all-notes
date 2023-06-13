# I/O Operations

### I/O Buffers

- Network programming is **all about organizing** *inter process communication* over some.
- **==Communication==** here implies exchange data between two or more processes.
- Process perform I/O operations, sending data to and receiving from participating processes.

---

- Involves **memory buffers** $\to$ *contiguous blocks* of memory allocated in **process address space** to *store data*.
- Any operation $\to$ data arrives $\to$ must be stored somewhere in the address space thus available for **further processing**.
- Before an *input operation* $\to$ buffer allocated $\to$ used as data destination point during the operation
  - After completion $\to$ buffer contains input data
- Similar is that of *output operation* $\to$ data prepped into some **output buffer** used in *output operation* where it plays role as **data source**

---

- These are essential for **network I/O** and thus critical for a **distributed application**.

### Synchronous and asynchronous I/O operations.

- Sync $\to$ block execution of thread, unblock only when finished
- async $\to$ associated with callback.
  - Provide flexibility but complicates code.
  - Simple initiation and does not block the thread.
  - Use thread to run other tasks while async runs in the background.

---

- `Boost.asio` implemented as framework , exploiting **==inversion of control==** approach.

  - After 1+ async ops are init $\to$ **application** hands over one of its **threads** of **execution** to the **library**

    - Uses thread to *run event loop* and *invoke* the **callbacks** provided by **application** to *notify* it about **completion** of *previously initiated async operation*.

- Results of **async operations** are passed to the **callback of the arguments**

### Additional operations

- Consider operations such as 
  1. Cancel async operation
  2. shutting down
  3. closing socket.
- Save resources by cancelling async operation.
- Shutting down socket useful if there is **need** for *one part* of the *distributed application*.
  - Informs the **other** part the *whole message* has been **sent**
    - Where the *application layer*  protocol does **not provide** us with other means to indicate the **message boundary** .
- Socket should be returned to OS when not required by the application, closing operation enables us to so.

## Using fixed length I/O buffers

- Fixed length used when size is known, thus allocated const char array on the stack.
  - May be **writable buffer** allocated in the **free memory**
    - Used as a **data destination point** when *reading data* from a *socket*.

---

- Use `boost::asio::mutable_buffer` and const version via `MutableBufferSequence` 
  - This represents a **collection** of the buffer objects.

- `Boost.asio` functions and methods that perform I/O operations accept objects that **satisfy requirements**
  - Either `MutableBufferSequence` or `ConstBufferSequence` as the **arguments** that *represent buffers*.

---

- Often just a **single buffer** involved , may implement more for **memory constrained environments**
- `std::vector<boost::asio::mutable_buffer>` class **satisfies** the *requirements* of `MutableBufferSequence` thus used as **represent a composite buffer**. 

---

- Create collection of buffer objects that could be **some container**.

---

- Boost offers simple methods with I/O related functions and methods.
- `asio::buffer()` accepts many variations of a **buffer** and *returns* an object of either `asio::mutable_buffers_1` or `asio::const_buffers_1` classes.
- These are **adapters** of the *mutable buffer / const buffers* 
- Providing an **interface** and **behaviour** that *satisfy the requirements* of the `MutableBufferSequence` and `ConstBufferSequence` methods
- Consider **two algorithms** and *corresponding code samples* to describe preparation of a memory buffer for `boost.asio` operations.

### Preparing buffer for output operation.

```c++
#include <iostream>
#include <boost/asio.hpp>

int main() {
	
	const std::string buf{"Hello"}; // buf is the raw buffer

	/*
		Creating buffer representation to satisfy ConstBufferSequence concept requirements.
	*/

	boost::asio::const_buffers_1 output_buf = boost::asio::buffer(buf);


	/*
		'output_buf' is the representation of the buffer 'buf' used in Boost.asio operations
	*/

	return 0;

}
```

### Preparing buffer for an input operations

```c++
#include <iostream>
#include <boost/asio.hpp>
#include <memory>
#include <array>



int main() {
	
	
	/*
		excpet to receive a block of data no more than 20 bytes long
	*/
	const size_t BUF_SIZE_BYTES = 20;

	/*
		Allocating the buffer
	*/



	/* copy string into this by calling (*buf).data() then using this as the location to fill in std::copy */
	std::unique_ptr<std::array<char, BUF_SIZE_BYTES>> buf = std::make_unique < std::array<char, BUF_SIZE_BYTES>>();
	

	/*
		create buffer representation that satsifies MutableBufferSequence concept requirements
	*/

	boost::asio::mutable_buffer input_buf = boost::asio::buffer(static_cast<void*>(buf.get()), BUF_SIZE_BYTES);
	std::cout << input_buf.data() << std::endl;

	/*
		'input buf' is the representation of the buffer `buf` used for boost.asio input operations.
	*/

}
```

## Writing to TCP synchronously

```c++
#include <iostream>
#include <boost/asio.hpp>

/*
	TCP socket is an output operation used to send data to the remote applicaition connected to this socket

	Sync writing is the simplest way to send data using socket provided by Boost.Asio.

	Methods / Functions that perform sync writing BLOCK THE THREAD and return only when data is written / error

	------------------------------------------

	use write_some from boost::asio::ip::tcp::socket

	template <class ConstBufferSequence>
	std::size_t write_some(const ConstBufferSequence& buffers)

*/

void writeToSocket(boost::asio::ip::tcp::socket& sock) {
	/*
		accepts reference to the socket object as an argument 

		Precondition is that the socket passed to it is already connected, otherwise the function fails.
	*/

	/* step 2 allocate filling buffer , basic ascii string to write to the socket.*/

	std::string buf = "Cunt";

	/*
		Store total number of bytes written to the socket. 
	*/
	std::size_t total_bytes_written = 0;

	/* step 3 Run the loop until all data is written to the socket 
		
		> Loop run > write_some() method is called except for degenerate case of an empty bufer.
		> At least one iteration of the loop is executed and write_some() method is called at least once.

		> evaluates to true only bytes written is equivalent to std::string object passed in.
	*/

	while (total_bytes_written != buf.length()) {
		std::cout << "write line" << std::endl;

		/* write_some returns number of bytes written in given sequence, we dont know how many bytes are 			   written each time. 
		*/
		total_bytes_written += sock.write_some(
			/* buffer denoted as the buf c string, size of data to read determined by difference in length */
			boost::asio::buffer(buf.c_str() + total_bytes_written,
			buf.length() - total_bytes_written)
		);
	}

}

int main() {
	
	/*
		Allocatees a socket, opens and synchronously connects to the remote application.

		Write socket function called and socket object passed as an argument

		Has try catch block intended to catch and handle expceptions that may be thrown Boost.Asio methods and 		   functions.
	*/

	std::string raw_ip_address = "127.0.0.1";
	unsigned short port_num = 3333;

	try {
		boost::asio::ip::tcp::endpoint ep(boost::asio::ip::address::from_string(raw_ip_address), port_num);

		boost::asio::io_context ios;

		/*
			step 1 > Allocating and opening socket
		*/

		boost::asio::ip::tcp::socket sock(ios, ep.protocol());

		//sock.connect(ep);

		writeToSocket(sock);
	}
	catch (boost::system::system_error& e) {
		std::cout << "Error occured! Error code = " << e.code() << ". Message: " << e.what();

		return e.code().value();
	}
	return 0;
}
```

### Alternative - send() method

- `boost::asio::ip::tcp::socket` contains another method called `send` for sync data sending.
- There are **three methods** , one of which is `write_some` 
- Has the **same functionality**, they are *synonyms* in this sense.

---

- Second overload accepts **one extra argument** 

```c++
template <class ConstBufferSequence>
std::size_t send(const ConstBufferSequence &buffers, 
                 socket_base::message_flags flags);
```

- named `flags` > specify a bit mask, representing flags that **control operation**

---

- Third overload is **equivalent** to second one but **does not throw exceptions** for case of failure
  - Instead the **error information** returned by **additional methods output** argument of `boost::system::error_code`. 

## Reading from TCP sync

```c++
#include <iostream>
#include <boost/asio.hpp>

/*
	read_some equivalent read operation for write_some

	read for write etc too.
*/

/*
	enhanced version to read entire buffer at once.
*/
std::string readFromSocketEnhanced(boost::asio::ip::tcp::socket& socket) {
	const unsigned char MESSAGE_SIZE = 7;

	std::array<char, MESSAGE_SIZE> buf;

	/*
		reads until the buffer is over.
	*/
	boost::asio::read(socket, boost::asio::buffer(buf, MESSAGE_SIZE));

	return std::string(buf.data(), MESSAGE_SIZE);

}

/*
	read some at a time
*/
std::string readFromSocket(boost::asio::ip::tcp::socket& sock) {
	const unsigned char MESSAGE_SIZE = 7;

	std::array<char, MESSAGE_SIZE> buf;

	std::size_t total_bytes_read = 0;

	/* keep reading until we have read the specified buffer input size thus entire message has been read */
	while (total_bytes_read != MESSAGE_SIZE){
		total_bytes_read += sock.read_some(
			boost::asio::buffer(buf.data() + total_bytes_read, MESSAGE_SIZE - total_bytes_read)
		);
	}

	/* instantiate a string with char array and specified bytes to read from this*/
	return std::string(buf.data(), total_bytes_read);
}

std::string readFromSocketDelim(boost::asio::ip::tcp::socket& socket) {
	boost::asio::streambuf buf;

	/*
		Sync read data from socket until '\n' encountered, we can specify any kind of delimter really.
	*/

	boost::asio::read_until(socket, buf, '\n');

	std::string message;

	/*
	* Buffer may contain other data after '\n' > parse buffer and extract only symbols before the delimiter
	*/

	std::istream input_stream{ &buf };

	std::getline(input_stream, message);

	return message;
}

int main() {
	
	std::string raw_ip_address = "127.0.0.1";
	unsigned short port_num = 3333;

	try {
		boost::asio::ip::tcp::endpoint ep(boost::asio::ip::address::from_string(raw_ip_address), port_num);

		boost::asio::io_context ios;

		boost::asio::ip::tcp::socket sock(ios, ep.protocol());

		sock.connect(ep);

		readFromSocket(sock);

	} catch (boost::system::system_error& e) {
		std::cout << e.code() << e.what();

		return e.code().value();
	}

	return 0;

}
```

### Alternative read_at function

- `boost::asio::read_at()` function provides a method to read data from a socket,.
  - Starting at a **certain offset**
    - This is *not often used*
- `boost::asio::read()` , `boost:asio::read_until()` , `boost::asio::read_at()` implemented similar method to the **original** `readFromSocket()` function in **our sample**.

## Write to TCP asynchronously.

```c++
#include <iostream>
#include <boost/asio.hpp>

/*

	writing is a flexible and efficient way to send data to remote application

	we view how to write data to a TCP socket async

	------------

	async_write_some method of boost::asio::ip::tcp::socket class can do this.

	this requires a suitable WriteHandler concept object of which is a functor

	void write_handler(
		const boost::system::error_code *ec,
		std::size_t bytes_transferred
	);

	ensures at least one byte is written during said async operation > may perform multiple times.
	
*/

struct Session {
	std::shared_ptr<boost::asio::ip::tcp::socket> sock;
	std::string buf;
	std::size_t total_bytes_written;
};

void callback(const boost::system::error_code& ec,
	std::size_t bytes_transferred,
	std::shared_ptr<Session> s)
{
	/*
		Checks whether the operatin was successful 
	*/
	if (ec.value() != 0) {
		std::cout << "error occurred  = " << ec.value()
		<< ". Message : " << ec.message();

		return;
	}

	/*
		increment the bytes written everytime callback is called again, using the total number in
		the previous async call.
	*/
	s->total_bytes_written += bytes_transferred;

	/*
		Cancel callback instantly once we have finished reading.
	*/
	if (s->total_bytes_written == s->buf.length()) {
		return;
	}

	/*
		If still data to consume, run the same operation recursively.
	*/

	s->sock->async_write_some(
		boost::asio::buffer(
			s->buf.c_str() +
			s->total_bytes_written,
			s->buf.length() - s->total_bytes_written
		),
		/* placeholders are called by async_write_some func(_1_var, _2_var) then we add 
			our new variable 
		*/
		std::bind(callback, std::placeholders::_1, std::placeholders::_2, s)
	);
}

/*
	Boost runs this async in another thread whilst our main thread continues.
*/


void writeToSocket(std::shared_ptr<boost::asio::ip::tcp::socket> sock){
	/*
		Allocate instance of session data in free memory.
	*/
	std::shared_ptr<Session> s{ new Session };

	/* step 4 . Allocating and filling the buffer */

	s->buf = std::string{ "Hello" };
	s->total_bytes_written = 0;
	s->sock = sock;

	/* step 5. Initating async write operation, this may not write everything 
	   therefore we pass in a callback

	   For this callback to work the session object must be on the heap as there is a recursive call.
	*/

	/*
		First arg 
			> A buffer containing data to write the socket.
			> Async buffer thus accessed by Boost.Asio at any moment between operation initiation and 
			  when callback is called.
			> Buffer must stay intact and available until callback is called
			> Guarantee this by storing it in a Session object, stored in free memory.
		Second arg
			> callback invoked when the async operation completes
			> Accepts two argument
				> First , specifies an error that occurs while operation executed
				> Second, specifies number of bytes written by the operation.

		We pass in an additional argument  , pointer to our session object
			> acting as context for this operation , use std::bind operation
			> Constructs a function object of which we attach our pointer 
			> This function object passed as a callback argument to the socket async_write_some
			

		async_write_some does not block thread of execution, initiates the writing operation and returns.
	*/
	s->sock->async_write_some(
		boost::asio::buffer(s->buf),
		std::bind(callback,
			std::placeholders::_1,
			std::placeholders::_2,
			s
		)
	);
}


/*
	run by a single thread, boost can create more threads for some internal operation.

	ensures no app code is executed in context of those threads.
*/
int main() {

	std::string raw_ip_address = "127.0.0.1";

	unsigned short port_num = 3333;

	try {
		boost::asio::ip::tcp::endpoint ep(boost::asio::ip::address::from_string(raw_ip_address), port_num);

		boost::asio::io_context ios;

		/* 
			Allocating , opening and connecting a socket
		*/

		std::shared_ptr<boost::asio::ip::tcp::socket> sock{ new boost::asio::ip::tcp::socket {ios, ep.protocol()} };

		sock->connect(ep);

		/*
			passing our pointer to the socket to write async.

			This initiates an async write operation and returns
		*/
		writeToSocket(sock);

		/* step 6 > keeps main function running while this is set
			this blocks as long as at least one pending async operation is happening.
			
			When final callback functions associated with async operations are completed this returns.
		*/
		ios.run();

	}
	catch (boost::system::system_error& e) {
		std::cout << e.code() << e.what();

		return e.code().value();
	}

	return 0;

}
```

### boost::asio::async_write

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220304184927350.png)

```c++
/* using async_write instead of write some */
struct SessionX {

	std::shared_ptr<boost::asio::ip::tcp::socket> sock;
	std::string buf;

};

/* only callback when the entire buffer has been drained of data using async_write*/
void callbackX(const boost::system::error_code& ec,
	std::size_t bytes_transferred, 
	std::shared_ptr<SessionX> s) {
	
	std::cout << "data from buffer has been drained" << std::endl;

}

void writeToSocket(std::shared_ptr<boost::asio::ip::tcp::socket> sock) {
	std::shared_ptr<SessionX> s{ new SessionX };

	boost::asio::async_write(sock, 
		boost::asio::buffer(s->buf),
		std::bind(
			callback,
			std::placeholders::_1,
			std::placeholders::_2,
			s
		)
	);

}
```

## Reading from TCP socket asynchronously.
