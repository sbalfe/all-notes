# Intro

**Task parallelism** - divide a single task into parts and run each in **parallel**.

**Data parallelism** - each thread performs the **same operation** on *different* parts of the data.

- Dont use concurrency when the benefit is not worth the cost
  1. more complex code
  2. higher risk of bugs
  3. harder to read and maintain.
- Use only when performance is greatly appreciated in the context of the application.
- Overhead in **thread launching** may mean its **not that high** of a **performance boost**.
  - Possibly even a decrease if one **thread** finishes before another, and depends on that one.
- They are **limited resources**
  - Too many at once can harm a program by **consuming more resources**.
- Exhausts memory as each one requires **seperate stack space**
- more threads $\to$ means more **context switching**

---

