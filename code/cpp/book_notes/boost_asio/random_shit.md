# Networking random shit

###### write vs write some

https://stackoverflow.com/questions/39517133/write-some-vs-write-boost-asio

###### async write > read

```c++
boost::asio::async_write(socket, boost::asio::buffer(sRequest.data(), sRequest.size()), [&](auto& e, auto bytes) {
			boost::asio::async_read(socket, response, [&](auto& e, auto bytes) {
				std::cout << &response; 
			});
		});
```

