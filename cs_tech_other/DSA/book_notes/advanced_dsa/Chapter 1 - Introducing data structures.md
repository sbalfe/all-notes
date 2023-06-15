# Improving Priority Queues: d - way heaps

## Handling Priority

- Handle a task with **priority**.

### Priority in practice - Bug Tracking

- One team for **one bug**
- Bug has **severity** in **number of days**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220131182828174.png)

- Resources limited $\to$ *prioritise bugs* 
  - Therefore some bugs are **more urgent than others**
    - Thus we need to **associate priorities** *with them*.
- Suppose some **developer** $\to$ team completes the **current task**
  - Asks for the **next bug** that must be **solved**
    - If this list was **static** $\to$ suite software could just sort the bugs once, return them in order.
- This is **wrong** as *new bugs are always added* thus must be **incorporated** into the **decision**
  - Bugs may **change priority also**.

## Solutions - Keeping Sorted List

- Update list every **insert** / **removal** / **modification**
  - Works well if these operations are **infrequent** and **list size** is *small*
- Requires **linear number of elements** to *change positions*
  - Both in **worst cases** and **average cases**.
- If there are **millions** of elements then we **would be in trouble**

### Sorted Lists $\to$ priority queues

- **Priority** queue keeps some **partial ordering** of the **elements** with a **guarantee** that the **next element** returned from the queue holds the **highest priority**.
  - We **gain performance** by *ditching* idea of *total ordering*
    - Each operations on the **queue** requires only **logarithmic time** now.
- Think about the problem and dont **overcomplicate** which could **reduce performance**.

## Describing Data Structure API - Priority Queues

- Each data structure may be broken into **lower level components**
- The **==API==** is some *contract* the **data structure** makes with **external clients** 
  - Includes **method definitions** alongside some **guarantees** on the **methods behaviour**
    - Provided in the **data structure specification**.
- *Priority Queue* provides:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220131191010446.png)

- **==Invariants==** $\to$ optional internal properties that always are true such as for a sorted list on the idea that every element is not greater than its successor.
  - Purpose is to make sure **conditions** *necessary* to live up to the contract with the **external clients** are *always met*
    - They are **internal counterparts** of the **guarantees** in the **API**.
- **==data models==** $\to$ To *host the data* 
  - This can be a **raw chunk of memory**
    - List
    - Tree
    - ....
- **==Algorithms==**
  - The **internal logic** that is used to **update** the *data structure* while making sure that the **invariants** are *not violated.*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220131191635104.png)

> - In appendix C we also clarify how there is a **difference *between an abstract data structure and concrete data structures***. 
> - The former includes the **API and invariants**, describing at a **high level** how clients will **interact** with it and the results and performance of operations. 
> - The latter **builds** on the **principles and API** expressed by the **abstract description,** adding a **concrete implementation** for its **structure and algorithms** (data model and algorithms).

- This is the **relationship** of *heaps / priority queues*
- **Priority queue** is an **abstract data structure** that may be implemented in many ways
- A **heap** is a *concrete implementation* of the **priority queue** using an **array** to **hold elements** and **specific algorithms** to **enforce invariants**.

### Priority queue at work

- C++ standard library contains the **implementation** for the **priority queue**.
  - This is a **==black box approach==** on relying on the **code**.
- Add the **bugs** to the **priority queue** in the **same order** we *did before*.
  - Assume we have **priority queue** containing those five elements
    - We *still dont know* the **internals** of the **priority queue** but may query it **via the API**,

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220131192705672.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220131193322033.png)

### Priority matters - generalise FIFO

- Regular queues but the **front of the queue** is *dynamically* determined based on **some kind** of *priority*.
  - The **differences** *caused* to the **implementation** by the *introduction* of *priority are **profound***
    - Enough to *deserve*  a **special kind of data structure**

- Define **basic containers** as *bag / stack* as *special* cases of **priority queues** 
  - Achieve better **performance** by *leveraging* their *specific characteristics*.

## Concrete Data Structures

- API knowledge of a **priority queue** not enough.
  - Especially on **time critical components** or **in data intensive applications**
    - Often need to **understand** the *internals* of the **data structures** and *details* of its **implementation** to ensure we **can integrate** it *in our solution* without **introducing** a *bottleneck*.

- Every **abstraction** needs to be **implemented** using a **concrete data structure**
  - Example $\to$ stack **implemented** using a *list / array* or theoretically a **heap**
- Choice of **underlying data structure** to implement a **container** only **influences** the **containers performance**
  - Choosing the **best implementation** is usually a **trade off**
    - Some *data structures speed* up some **operations** but make **other operations slower**.

### Comparing performance

- Consider **three naive** *alternatives* using **core data structures**
  1. Unsorted array.
  2. Where we just add elements to its end.
  3. Sorted array.
- Ensure the **ordering** is *reinstated* every time we **add a new element**
- Balanced trees where **heaps** are a **special case**
  - Lets compare, in table 2.2 $\to$ **running times** for *basic operations* implemented with **these data structures**.

### Whats the right concrete data structure.

- *naive* choice lead to a **linear** requirement for at **least one of the core operations**
- A *balanced tree* would always **guarantee** a *logarithimic case*
- The *linear time* is often regarded as **feasible**
  - Still a difference between **logarithmic** and **linear** 
    - For **one billion elements** $\to$ difference between **1 billion operations** and a **few dozens**
- If each **operation** takes **1 millisecond** 
  - Thus meaning it takes **11 days** to *less than a second.*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220131223010227.png)

- Moreover **consider** that **most of the times containers**
  - **Priority's queues** in *particular* are used **as support structures**.
    - Thus they are **part** of the **more complex algorithms**.
  
  