# Server Applications

- Passive
- Push > send request to client without them specifying.
- **iterative servers** $\to$ serves client in **one by one fashion**.
- **parallel servers** $\to$ serves **multiple clients** in *parallel*.
- single processor > can pseudo parallelise by switching between sub operations of each client needs.
- **synchronous server** $\to$ uses *synchronous socket API* calls to **block** the *thread of execution* until the requested operation is complete.
  - Accept methods such as `boost::...::accept()` and `boost::...read_some()` to *receive request* from client and `write_some` to write.
  - All are **blocking methods**.
- **asynchronous server** $\to$ is *non blocking*
  - *async_accept()*
  - `async_read_some()`
  - `async_write_some()` 
  - `async_write()` 