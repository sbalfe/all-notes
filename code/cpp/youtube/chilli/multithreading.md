# Multithreading Categories



## Bulk parallel processing

- Split our dataset into **4 seperate** threads for **massive speed increase**.
  - Only if out data is in certain **ways**.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119045509834.png)

- The datasets **dont communicate** in this *example*.

---

- Must be **perfectly parallel** for this to work
- most of the time we combine the *results* / share memory space/ communicate with one another using **synchronisation**

---

- If the **datasets** are *uneven* in **size** then one thread finishes much **earlier**.
  - Often involves queues/ dispatching to other threads etc.

---

- We wish to **maximise our CPU performance** by using the **threads**. 
- When we **group** together to increase performance this **==bulk parallel processing==**

---

## Asynchronous issues (example: sockets programming)

- Lots of work in sockets involves **waiting on network**, this **is a significant chunk** most often
- Therefore we can use **polling** to stop the **blocking function** that does the **network communication**.
- We poll the output every time frame to see if it has returned, this is generally **complex** especially for **large** **situations**.
- This can be fixed using **coroutines** 

<img src="https://miro.medium.com/max/1838/1*4116tixMpWo4vngINfzcDA.png" alt="Coroutines: Suspending State Machines | by Garima Jain | Google Developers  Experts | Medium" style="zoom:50%;" />

- We alternatively say when the packet comes we process with some function that we have given it
  - So a *callback* as with **javascript**.
- Socket has many modes > blocking / callback.

---

- Alternatively we can **use a thread** instead.
  - Main > thread called > runs our server comms function A() B() C() D().
  - Calls **receive** which is a **blocking function** however as its a thread it *doesnt stop our main code.* 
- So rather than **callbacks  /state machines** we can just let the **operating system** manage that for us.

## Needy Knob (buffer that needs to fill forever)

- Example is audio playing from data in a buffer.
- This can be stalled in processing which messes up and destroys the sound output as no work is moving data from the buffer to the output.
- must keep small buffers that require lots of filling such as using **threads**.

## Organisational

- Lots of **seperate systems** that are not required **together**.
- Lots of games have multithreading but are just offloading small threads to physics / sounds etc.
- example is UI > run on seperate thread
  - In complex calculations it freezes the ui, therefore splitting into threads ensures the system is **safe** and **smooth all the time.**

