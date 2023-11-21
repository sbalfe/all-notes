# Back to Basics https://www.youtube.com/watch?v=pfIC-kle4b0&t=46s

- Can concurrently and execute multiple stacks.
  - Two threads is **not twice the performance**
	  - 

---

- Performance is the **concurrency of computing**. 

---

- Concurrency meaning things happen at once , order matters and sometimes takes time to wait on shared resources.

- parallelism - everything happens at once, instantaneously. 

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220128033609936.png)

---

## Neccessity of concurrency.

- Concurrency used as its required to function
	-   Orchestra
	- subway transit
  - traffic stop
  - memory allocator
  - file I / O
  - Network requests (awaiting data)
- Concurrency is motivated by **increased peformance**
  - Potential for more tasks to occur at once and thus increases performance
    - Often if we have multiple cores on our machine.

---

## Thread - Mechanism for concurrency

- Enables execution of two control flows at same time
- main thread is where the program starts
- may have 1+ threads
  - Execute a block of code
  - Execute other functions
  - overall - sharing the same code, same data
    - All while main still execute

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220128034302059.png)

- Threads run in one process.

### High level view of Thread

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220128034502426.png)

- Thread ID, Control flow, stack for local variables.

### When to use threads.

- Heavy computations such as in graphics
- Resolve complex calculations
- GPU have 100 / 1000s of threads good for massively parallel tasks.
- Separate work
  - Gain performance
  - Simplifies the logic of a problem.

### C++ std::thread library

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220128034857626.png)

```c++
#include <iostream>
#include <thread>

void test(int x){
    std::cout << "Hello from our thread" << std::endl;
    std::cout << "Argument passed in: " << std::endl;
    
}
```

