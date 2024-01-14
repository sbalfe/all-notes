# Architecture (async http server with io uring and c sockets)

- Main thread for initial setup.
- I/O Threads (handle async operations using io_uring and coroutines) [one async loop + per thread]
  - Accepting clients ( `io_uring_prep_accept()` )
  - Reading HTTP requests from clients ( `io_uring_prep_recv()` )
    - Offload task of parsing HTTP to the thread pool (`io_uring_prep_send()`)
  - Writing HTTP responses.
    - Offload task of building HTTP to the thread pool 
  
  > ==How to communicate between I/O threads and Thread pool ?== 
- Thread pool (handle CPU bound tasks) [shared execution pool between each async I/O thread]
  - Parsing HTTP requests
  - Building HTTP responses

---

- X cores active
  - each core has an async accept loop `io_uring_prep_multishot_accept()`
    - each async accept loop can spawn a new coroutine to handle the request.
      - each new spawned coroutine *may* use the thread pool for CPU bound tasks.
  
- Spawning coroutines, io_context, scheduling

  - Initiator -> the thread running the async accept loop would *spawn* the initial coroutine.

  - Async op processor -> thread running the coroutine that calls the OS (async operation)

  - Async event demultiplexer -> read the completed events from the queue

  - Completion event queue -> 

  - Proactor -> thread that is passed events from the queue.

    

