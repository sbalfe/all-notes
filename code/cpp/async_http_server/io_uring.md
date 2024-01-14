# io_uring

> Resources
>
> - [Efficient IO with io_uring](https://kernel.dk/io_uring.pdf)
> - [Lord of the io_uring](https://unixism.net/loti/)
> - [Low level io_uring](https://unixism.net/loti/low_level.html#low-level)
> - [Submission queue polling](https://unixism.net/loti/tutorial/sq_poll.html#sq-poll)
> - [io_uring and networking in 2023](https://github.com/axboe/liburing/wiki/io_uring-and-networking-in-2023)
> - [A universal IO abstraction for C++](https://cor3ntin.github.io/posts/iouring/)
> - Library reference
>   - [io_uring_prep_nop](https://unixism.net/loti/ref-liburing/submission.html#c.io_uring_prep_nop)
> - [uring cmake](https://github.com/facebook/folly/blob/main/CMake/FindLibUring.cmake)

![Missing Manuals - io_uring worker pool](https://blog.cloudflare.com/content/images/2022/02/image3-8.png)

### What is io_uring

- An asynchronous I/O API for Linux.
- Provide an API without the limitations of the current `select` , `epoll`, `poll`, `aio` system calls.
- Has a very low performance overhead, essential for anyone utilising asynchronous programming models.

### io_uring interface

- The interface uses **ring buffers** as the main interface for kernel-user space communication.
- There are system calls, but are vastly minimised. We can use a polling mode to reduce this.

##### Mental model

<img src="https://developers.redhat.com/sites/default/files/uring_0.png" alt="Why you should use io_uring for network I/O | Red Hat Developer" style="zoom:50%;" />

- There are **2 ring buffers**:
  1. One for submission request =  **submission queue (SQ)**.
  2. Another that informs you on the completion of those request = **completion queue (CQ)**
- The buffers are *shared* between the kernel and user space. 
  - Use `io_uring_setup()` to set these up.
  - Map them to the user space space with 2 `mmap(2)` calls.
- We must tell io_uring what to get done (file read/write, accept client connections, etc...).
  - These are described as part of the **submission queue entry (SQE)**.
  - Append to the tail of the **submission ring buffer**.
- Tell the kernel via `io_uring_enter()` system call that we have added an **SQE** to the **submission queue ring buffer**.
  - We are able to add multiple **SQEs** before making this system call.
  - `io_uring_enter()` can also wait for a number of requests to be processed by the kernel before it returns, this ensures we are ready to read off the **completion queue** for results.
- The kernel processes requests submitted and adds **completion queue events (CQE)** to the tail of the completion queue ring buffer.
  - We can read **CQEs** off the head of the **completion queue ring buffer**. There is one **CQE** corresponding to each **SQE** and it contains the status of the particular request.
- We keep adding **SQEs** and reaping **CQEs** as we require.
- We have a **polling mode** available too. This is where the kernel polls for new entries in the submission queue.
  - This **avoids system call** overhead of calling `io_uring_enter()` every time you submit entries for processing.

### io_uring performance

- io_uring can be a **zero-copy system** because of the shared ring buffers between the kernel and user space.
- Byte copying is necessary for system calls between user and kernel space. Most io_uring communication is via **shared buffers** between the user and kernel space, this overhead is avoided completely.
- In high performance applications, syscalls eventually build up to be a huge overhead overtime.
- There are other overheads such as **spectre and meltdown** workaround that are *non trivial*.
- So avoiding system calls is essential for high performance applications.

---

- When using sync programming and async programming interfaces under Linux there is *at least one sys call* in the **submission** of each request.
- in `io_uring` you can **add several requests** by simply adding **multiple SQEs** each describing the I/O operation you want and make a single call to `io_uring_enter`.
- We can have the kernel **poll** and pick up your **SQEs**. For high performance applications, this means even lesser system call overheads

---

- Shared ring buffers being used effectively can mean `io_uring` performance is memory bound.

> Memory bound refers to **a situation in which the time to complete a given computational problem  is decided primarily by the amount of free memory required to hold the  working data**.

- `io_uring` = 1.7M 4k IOPS (4 kilobyte input output operations per second) whereas `aio` managed 608k. This is not *fair* really as aio does not support polled mode, but even then `io_uring` is 1.2m IOPS.

---

- The raw throughput of `io_uring` is achieved by viewing a *no-op* request type.
- This achieves 20M messages per second

### Reference

##### `io_uring_queue_init` 

> https://man.archlinux.org/man/io_uring_queue_init.3.en

- The [io_uring_queue_init(3)](https://man.archlinux.org/man/io_uring_queue_init.3.en) function executes the  [io_uring_setup(2)](https://man.archlinux.org/man/io_uring_setup.2.en) system call to initialise the submission and  completion queues in the kernel with at least *entries* entries in the submission queue and then maps the resulting file descriptor to memory    shared between the application and the kernel.

##### `io_uring_prep_accept`

> https://man7.org/linux/man-pages/man3/io_uring_prep_accept_direct.3.html

- Prepare an accept request.
- pass the SQE, server socket, client address + len, 0.

##### `io_uring_sqe_set_data`

> https://man7.org/linux/man-pages/man3/io_uring_sqe_set_data.3.html

- Takes in the SQE and a void pointer to some user data.

- Sets user data for submission queue event
- Stores a user data pointer pointer with the submission queue entry sqe.

##### `io_uring_submit`

> https://man7.org/linux/man-pages/man3/io_uring_submit.3.html

- Takes in our ring as parameter and submits all previous request.
- Our previous request are submitted to the submission queue.

##### `io_uring_wait_cqe`

- Waits on our requests we submitted.
- Essentially unblocks as soon as available.

##### `io_uring_cqe_seen`

- Mark this request as seen.
- Takes in our ring + completion queue event

##### `io_uring_prep_readv`

- prep a readv

##### `io_uring_prep_recv`

- Prepare a receive request

##### `io_uring_prep_send`

- Prepare a send request

##### `io_uring_submit_and_wait`

- /

##### `io_uring_enter`

> https://manpages.debian.org/unstable/liburing-dev/io_uring_enter.2.en.html

- [io_uring_enter(2)](https://manpages.debian.org/unstable/liburing-dev/io_uring_enter.2.en.html) is used to initiate and complete I/O    using the shared submission and completion queues setup by a call to    [io_uring_setup(2)](https://manpages.debian.org/unstable/liburing-dev/io_uring_setup.2.en.html). A single call can both submit new I/O and wait for    completions of I/O initiated by this call or previous calls to    [io_uring_enter(2)](https://manpages.debian.org/unstable/liburing-dev/io_uring_enter.2.en.html).

##### `io_uring_get_sqe(3)`

> https://man7.org/linux/man-pages/man3/io_uring_get_sqe.3.html

- get the next available submission queue entry from the submission queue

##### `io_uring_prep_multishot_accept()`

- /


### io_uring and networking in 2023

> https://github.com/axboe/liburing/wiki/io_uring-and-networking-in-2023

- As an IO model, **io_uring is applicable to both storage and networking  applications**. In UNIX, it’s often touted that *“everything is a file”*.  And while that is (mostly) true, once you have to do IO to the file,  then all files are not created equal and must in fact be treated  differently.
- With its **completion model based design**, converting storage applications to use io_uring is trivial, and can be done in steps:
  1. Initial conversion from eg libaio to **io_uring**.
  2. Honing the conversion by utilising applicable advanced features that are exclusive to io_uring
- For networking, the path to **idiomatic** and **efficient** io_uring is a bit more involved.
- Network applications have been written with a **readiness** type of model for decades, most commonly using `epoll(2)` these days to **get notified when a given socket has data available**.

- While these applications can be **adapted to io_uring** by swapping **epoll  notifiers with io_uring notifiers**, going down that path **does not lead to an outcome** that fully takes **advantage** of what io_uring offers. It can reduce system calls compared to epoll, but will not be able to take advantage of some of the other features that io_uring offers. We need a change to the IO event loop.

#### Batching

- One of the key benefits of **io_uring** is that multiple actions can be  **completed in a single system call**. This is readily apparent in how retrieving and preparing a new **SQE (submission queue entry)** for **submission** (`io_uring_get_sqe()` + `io_uring_prep_foo(sqe)`) is done **separately** from informing the kernel (`io_uring_submit()`) that new requests are available for processing.
- The io_uring system call for **submitting new IO**, `io_uring_enter(2)` , also  **supports waiting for completions at the same time**.
  - This allows **IO that completes synchronously to be done efficiently**, compared to **async APIs that separate submission** and  **wait-for-completion** into **two different operations**. 
  - `liburing` has support  for this through the `io_uring_submit_and_wait()` helper,  **allowing an application** to not only **batch submissions**, but also **combine  submissions** and completions into a **single system call**.
- This comes in handy in **IO event loops**, where **reaping completions** will  often **yield new operations** that need to be submitted. An application can **process these completions** while **simultaneously grabbing new SQEs** and  **preparing them for submission**, and finally **run another**  `io_uring_submit_and_wait()` to **repeat the event loop**.

### Multi-shot

- By default, any **SQE submitted** will **yield a single completion event**, a **CQE**.
  - For example, an **application submits a read**, and once that **read completes**, a **completion event** is **posted** and this **completes the  operation**. However, **io_uring** also supports **multi-shot requests**. 
  - These types of requests are **submitted once**, and will post a **completion** whenever the operation is triggered.

- Consider a **network application that accepts new connections**. The application could **retrieve an SQE** and use `io_uring_prep_accept()` to be **notified when a connection request is received**. 
  - Once the  **connection request comes in**, a **completion** is **posted** with the **file  descriptor**, and now the application has to **submit a new accept SQE** to  handle future connections.
- The application could also choose to **utilize multi-shot** accept by instead using `io_uring_prep_multishot_accept()` to prepare the request. 
  - By doing so, that informs **io_uring** that it  **should not finish the request fully** once a **single connection has been  accepted**. Rather, it should **keep it active** and **post a CQE whenever a new connection request comes** in. This reduces the housekeeping the  application needs to do when handling new connections. 

- By default, multi-shot requests will remain active until:
  1. They get **cancelled explicitly** by the **application** (eg using `io_uring_prep_cancel()` and friends), or
  2. The request itself experiences an error.

- If **further notifications** are expected from a **multi-shot request**, the CQE completion will have `IORING_CQE_F_MORE` set in the `flags` member of the `io_uring_cqe` structure. 
  - If this **flag isn’t set**, the **application must re-arm** this  request by submitting a new one. As long as that flag is set on **received CQEs for a request**, the application can **expect more completions** from  the **original SQE submission**. In general, **multi-shot requests** will  **persist until they experience an error**.


- Multishot is useful also for **receiving too** . Multiple requests are expected, then a multi shot variant of the receive operation is also applicable. Rather than use `io_using_prep_recv()`, use `io_uring_prep_recv_multishot()` that prepares an SQE for **multi-shot receive**. Just like accept, a **multi-shot** receive is submitted once, and **completions are posted** whenever **data arrives** on this socket. The astute reader may now be wondering
  - There is **provided buffers** which is where the data received will be placed.
- We can use **multi-shot** operations for **poll requests** also, the concept is identical - arm some poll request once, and get a notification whenever the specified pool mask becomes true

#### Provided buffers

- A **readiness based IO model** has the distinct advantage of **providing an  opportune moment** to provide a buffer for **receiving data** - once a  **readiness notification arrives** on a given socket, **a suitable buffer can  be picked** and transfer of data can be started. 
  - This **isn’t true with a  completion based model**, as a **receive operation is submitted ahead of  time**. 
  - Obviously a **buffer can be picked by the application** when the  receive is submitted, but for applications handling hundreds of  thousands of requests at the time, this doesn’t scale very well and  leads to excessive memory consumption.

### Socket state

- By default, io_uring will **attempt a receive operation** when submitted. If **no data is available**, it’ll **rely on an internal poll implementation** to  get **notified when data is available**. 
  - To **aid the application** in helping  **io_uring make more informed decisions** on **when to receive,** io_uring will  t**ell the application if a given socket** had **more data** to be read, and  **similarly allow the application** to control whether to **go directly to  internal poll** rather than **attempt a receive upfront**. 
  - This can **help  increase efficiency** by **not attempting a receive** on a **socket** that is  **presumed** (or most likely) **empty**.

- When a **CQE is posted** for **any of the receive variants** that io_uring  supports, the flags member also contains information on if the socket  was empty after the receive had been completed. 
  - If `IORING_CQE_F_SOCK_NONEMPTY` is set in the cqe `flags`, then the **socket had more data available post receive**. 
  - This may be because the **receive asked for less data** than **was originally available**,  or perhaps **more data arrived** after the **receive was submitted**.

- Conversely, when **submitting a receive operation**, if the socket is  **assumed currently empty**, then it is pointless to first attempt a receive and then fall back to internal poll. For this case, the application may set `IORING_RECVSEND_POLL_FIRST` in the SQE `ioprio` member to tell io_uring to not bother with attempting the send/receive  initially, rather it should go directly to internal poll and wait for  notification on when data/space is available.
