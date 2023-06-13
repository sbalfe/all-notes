# 1 - WinMain

```c++
NDEBUG;_MBCS // preprocessor switch to tell std that we are in release mode, std for standard library.

    // useful in regards to the assert macro.
```

```c++
Multi-threaded Debug (/MTd) // static linking in code generation, to ensure the visual studio runtime is not required for the execution  
```

```c++
Fast (/fp:fast) // fast for non exact float point calculations 
```

```c++
Windows (/SUBSYSTEM:WINDOWS) // this is the subsystem we require for making windows applications
```

---

## Windows

- Class from windows
- can create instances of this.
- Not C++, all windows sys calls.
- call a class function to create instance on windows
- Then call another function to create instances of the class

---

- Window class required as everything is a window basically. 
- Classes for buttons , labels , etc.... 
- using direct3D > just requires us to make a single class. 

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210809183155053.png)

### Message Loop / WndProc

- Messages occur on user interaction like clicking on the window
- example in notepad it redraws UI when entering it.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210809205842558.png)

- Windows message are just events
- Two sides are the WIN32 and app side. 
- windows has a lot of data to handle
  - One of which , is the message queue, which are events from the user etc.

- events are placed in the queue in the order they come in
- call a get message function to take one of the queue 
  - Perform some processing on this message 
- translate message can translate the incoming message to some command such as key down pressed 
  - This creates a new message which is placed on queue.

- Dispatch message , goes back to win32

- all windows > have pointer to windows procedure 
  - this defines the appearance and behaviour therefore
- dispatch > checks the procedure to use then passes that message to the specified procedure 
  - Processes the message > then calls default windows procedure. 
  - DefWndProc $\to$​ many messages, often just apply the default windows process.
  - When you dont want default behaviour you add upon that.

https://stackoverflow.com/questions/47117509/high-order-and-low-order-byte#:~:text=The%20high%2Dorder%20byte%20would,that%20in%20hex%20as%200x147B. - low and high order bytes.

### Messages

- NC stands for **non client **
  - This is all the border stuff like X button, borders etc.

---



| kEY        | lPARAM                                                       | wPARAM                                               |
| ---------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| WM_KEYDOWN | Repeat count , scan code, extended key flag, previous key state flag, transistion state flag | virtual key codes (what keyboard button was pressed) |
|            |                                                              |                                                      |
|            |                                                              |                                                      |

https://docs.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes - virtual key code list

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210810164717310.png)

use this to draw your own icon.