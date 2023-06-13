# Managing Threads

## Basic thread management

- Each **C++** program has **at least one thread** 
- Started by **C++ runtime** 
  - thread running **main()**
- launch more threads > have **function as entry point**

---

- New threads run **concurrently** 
- **entry points** *returns* $\to$ **thread exits**
- std::thread can **be waited for** in its finishing

### Launching a thread

- construct a `std::thread` object to specify the **task to run** on that **thread**
- Task could be some  **functor** 
  - Possibly taking **additional parameters**
  - with series of **independent operations** that are *specified* via **messaging system**
- Stops when its **signalled**  to do.

```c++
void do_some_work();
std::thread my_thread(do_some_work);
```

- must include `<thread>` header so the compiler is aware of this definition. 

- `std::thread` works with any callable type 
  - can pass an **instance** of some class with a **function call operator**

```c++
class background_task
{
public:
    void operator()() const
    {
        do_something();
        do_something_else();
    }
};
background_task f;
std::thread my_thread(f);
```

- supplied function $\to$ copied to **storage** belonging to *newly created* thread of **execution** 
  - Then **invoked** from there
- Passing a **function** object  $\to$ *thread constructor* avoids C++ **most vexing parse**
  - Passing some **temporary** rather than **named** 
    - Syntax similar as that of **function declaration**
      - The compiler **interprets it like this as such.**

```c++
/* 
	declares a my_thread function taking single parameter
	
	type > pointer-to-a-function-taking-no-params-and-returning-background-task-object
	
	returns std::thread rather than launching a new thread.
*/

std::thread my_thread(background_task());
```

- Avoid by just naming it properly you fucking retard.
- use **extra ( )** 
- or use the **uniform initialization syntax** 
  - Example:

```c++
std::thread my_thread((background_task())); /* prevent function interpretation  */
std::thread my_thread{background_task()}; /* uniform init syntax , declares a variable */
```

- Avoid using **lambda** *expression* 
  - New **feature** from `C++11` to write some **local function** 
    - Possibly capture the **local variables** and prevent **arguments required**

```c++
std::thread my_thread([]{
    do_something();
    do_something_else();
});
```

- Decide to wait for its **finish**
  - Or leave it to **run on its own**
    - via detachment 

- If **undecided** before `std::thread` object destroyed

  - program is **terminated** as the `std::thread` **destructor** calls `std::terminate()` 

    > https://akrzemi1.wordpress.com/2011/09/28/who-calls-stdterminate/#

    >  https://leimao.github.io/blog/CPP-Ensure-Join-Detach-Before-Thread-Destruction/

- Ensure its correctly **joined / detached** 
  - Even if **exceptions are present**.
- This is just a choice **before** the `std::thread` object is **destroyed** 
  - It may have **finished** a *while* before the **join / detach** 
    - If detached $\to$ if **thread is still running** , it will of course **continue**.
    - May run for a **while** after `std::thread` is destroyed.

- Thread only ends when it **returns** from the **thread function** after *detachment* 

---

- If you are **not waiting**
  - Ensure the data **accessed** is **valid** for the **lifetime** of the **thread** 
- Same problem with **single threaded code**
  - Causes **undefined behaviour** when accessing an **object** after **destruction**
    - Use of **threads** provides **more opportunity** to confront this **issue**.

---

- One such problem $\to$ when the **thread function** holds **pointers/references** to *local variables*
  - along with the *thread* **not having finished** when the **function exits**
- Following listing shows example of **such scenario**

```c++
struct func
{
    int& i;
    func(int& i_):i(i_){}
    void operator()()
    {
        for(unsigned j=0;j<1000000;++j)
        {
            do_something(i);        1
        }
    }
};
void oops()
{
    int some_local_state=0;
    func my_func(some_local_state);
    std::thread my_thread(my_func);
    my_thread.detach();             2
}                                   3
```

- $1$ - Potential access to **dangling reference**
- $2$  - **Do not** wait for **thread** to **finish**
- $3$ - The **new thread** could **still be running**

---

- The **new thread** associated with ` my_thread` 
  - Still running when `oops` exits.
    - As you have **detached** when it was **not done yet**
  - If still running 
    - scenario as follows 
      1. Next call to `do_something(i)` accesses a **destroyed variable**
         - Same as **single threaded**
      2. Allowing a **pointer** / **reference** to a *local variable* **to persist** beyond *function exit*
         - Is ==never a good idea== 
         - Easier to make **this mistake** with **multithreaded code**
         - Not always apparent what was **happened**

---

**Table 2.1. Accessing a local variable with a detached thread after it has been destroyed**

| Main thread                                           | New thread                                                   |
| :---------------------------------------------------- | :----------------------------------------------------------- |
| Constructs my_func with reference to some_local_state |                                                              |
| Starts new thread my_thread                           |                                                              |
|                                                       | *Started*                                                    |
|                                                       | Calls func::operator()                                       |
| Detaches my_thread                                    | Running func::operator(); may call do_something with reference to some_local_state |
| Destroys some_local_state                             | Still running                                                |
| Exits oops                                            | Still running func::operator(); may call do_something with reference to some_local_state => undefined behavior |

- Possible solution is **making the thread self contained** 
  - *Copy* the data **into the thread** rather than **sharing the data**
    - using a **callable object** for the *thread* function $\to$ this is **copied** to the thread
      - Therefore the **original object** can **immediately be destroyed.**
- Must consider the **objects** that contain **pointers / references** 
  - **Bad idea** to create **threads** with a *function* that has *access* to some **local variable** in the **function**
    - Unless **thread** is **guaranteed** to *finish* before the **function exits**.
- Alternatively just **join** the **thread**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210818234151155.png)

### Waiting for a thread to complete

- **Wait** for some thread $\to$​ call `join()` on the correct `std::thread` instance.
- For the diagram above , replace `.detach()` to `.join()` is **sufficient** to ensure the **thread** was **finished** before the function *exited* , thus **before** the **local variables** were **destroyed**
- The **thread call** in this case is **pointless** as the **current thread** is *not doing fuck all*.
  - It would normally **have work** or **launch more threads** and *wait* on them.

---

- `.join()` $\to$ is a **simple** yet **brute force** technique $\to$ either **wait** on *thread* or *not*

  > Condition variables /  futures offer **fine grained control** on *time control* > chapter 4.

- `.join()` $\to$​ **cleans** any **storage** associated with the thread $\to$​ `std::thread` object **no longer associated** with the **finished thread**, it has **no associations** to *any thread*

- Therefore `.join()` can **only be called once** for a *single thread*.

  - Once called $\to$ `.joinable()` in the thread $\to$ *returns* **false**

---

### Waiting in exceptional circumstances

- Must call `.join()` or `.detach()` **before** `std::thread` object is **destroyed**.
- Often call `.detach()` **immediately** after the thread starts $\to$ **removes that problem**
- If **you must wait** $\to$ carefully *select the location* of `.join()` 
  - The **call** to `.join()` is **liable** to be **skipped** if some **exception** is thrown after the **thread starts** but **before** the `.join()` call.

---

- To **prevent termination** when **exceptions** are thrown
  - Make a **decision** on *what to do* in this case.
- If you **intend** to call `.join()` in the **non-exceptional case** must also **call** for the **exceptional case**.
  - *avoids accidental lifetime problems*.
- Code to display this issue.

```c++
struct func;

void f(){
    int someLocalState = 0;
    func myFunc(someLocalState);
    std::thread t(myFunc);
    try {
        doSomethingInMainThread();
    }
    catch(...){
        t.join(); //1 - join by exception
        throw; // exits after throwing exception
    }
    t.join(); // 2 - join normally
    
}
```

- Using a `try/catch` **block** to ensure that the **thread** *joins* in **both cases**.

- `try/catch` is **verbose** $\to$ *not ideal*.

- If **important** to make sure the **thread completes** *before* the *function exits*

  - Either due to:

    1. Has some **reference** to *local variable*
    2. Other reasons...

  - Then its **vital** to ensure this is the **case** for *all exit paths*

    - Normal 

    or

    - Exceptional.

- Provide a **concise** mechanism using **RAII** (*Resource acquisition is initialisation*)

#### Using RAII to wait for a thread to complete.

```c++
class thread_guard{
    std::thread& t;
    
    public: 
    	explicit thread_guard(std::thread& t_): t(t_){}
    
    	
    	~thread_guard(){
            if(t.joinable()){ # // - only join if this is false ofc
                t.join(); // 2 - joins thread on guard destruction such as end of f.
            }
        }
    
    	/* 3 - Delete these as not required , makes them unkown to compiler.
            copying or assigning this is dangerous, may outlive the scope of the
    		thread it was joining , any attempt to copy generates compiler errror.
    	*/
    	thread_guard(thread_guard const&) =delete;
    	thread_guard& operator=(thread_guard const&) =delete;
};

struct func; # defined earlier
void f(){
    int someLocalState = 0;
    func myFunc(someLocalState);
    std::thread t(myFunc);
    thread_guard g(t);
    doSomeThingInMainThread(); #4 - objects are destroyed in reverse order at end of f
}
```

- The `thread_guard` object is **destroyed** first via `#4` 
- Thread is **joined** even if `doSomeThingInMainThread()` throws an **exception** for the case of the function **exiting**
- If you **do not** need to **wait** for a **thread to finish**
  - Just **avoid** this *exception safety* by **detaching**.
    - This **breaks** the *association* of the **thread** with the `std::thread` **object**. 
      - Ensures `std::terminate()` not called on `std::thread` **destruction**.

### Running Thread in the background

- Call `.detach()` on `std::thread` to allow it to **run** in the **background**
  - Leaves **no mean** of *communication* with it.
- You **cannot** call `.join()` as you have lost `std::thread` that **references it**.
  - Therefore you **cannot** *wait* for its **completion**.
- Detaching threads $\to$ *truly* run in the **background**
  - The *ownership / control* $\to$ handed to **C++** *runtime library*.
    - Ensure the **resources** with *the thread* are **reclaimed correctly** when the **thread exits**

---

- **Detached threads** $\to$ often called **daemon threads** $\to$ as in *UNIX* *daemon process* that runs in **background** with **no explicit UI**

- Often **long running**

- Perform Various **background tasks** such as:

  1. Monitoring the **filesystem**
  2. Clearing **unused** *entries* out of **object caches**
  3. **Optimize** *data structures*.

- Extreme $\to$ **make sense** to use a **detached threads** when theres **another mechanism**

  - That *identifies* when the **thread** has **completed**.

  OR

  - When the **thread** used for a *fire / forget* task.

---

- Calling `.detach()` **removes** ability to be **joinable**

```c++
std::thread t(do_background_work);
t.detach();
assert(!t.joinable());
```

---

- Can **not call** `.detach()` on `std::thread` with **no associated thread of execution** , same idea with `.join()`  
- Can only call `.detach()` if `.joinable()` is **true**.

---

#### Word processor example

- Can **edit** many documents at **once**
  - Many ways, common $\to$​ **multiple** *top level windows* that are **independent**.
    - One for **every document** that is being **edited**.
  - Appear to be **seperate**, but are running under **same application instance**.
- Handle by **firing a thread** for every **new document**, but with **different data**.
  - Each thread is **contained** and *does not care* of the **other threads** being executed for **seperate tasks**.

```c++
void edit_document(std::string const& filename)
{
     // display new document 
     open_document_and_display_gui(filename);
    
     // while they have not exited
     while(!done_editing())
     {
         // check for user input
         user_command cmd=get_user_input();
         
         // Want to create a new document
         if(cmd.type==open_new_document)
     	{
             // ask for filename from user 
             std::string const new_name=get_filename_from_user();
             
             // start a new thread , based on this function, and pass unique filename
             std::thread t(edit_document,new_name);
             
             // allow this to run in background.
         	 t.detach();
 		}
 		else
 		{
 			// whatever else the user wanted to do.
            process_user_input(cmd);
 		}
 	}
    // closed the document 
}
```

### Passing arguments to a thread function.

- By default, using the **argument passing** method shown above
  - They are **copied** to **internal storage** to be accessed by the **new thread of execution**.
    - Occurs even if **the function** *expects* a **reference** 

---

```c++
void f(int i,std::string const& s);
std::thread t(f,3,"hello"); 
```

- Creates a **new thread** associated with `t` $\to$ calls `f(3, "hello")`
- `f` takes a `std::string` as the **second parameter** 
  - String **literal** passed as a `const char*` $\to$ converted $\to$ `std::string` only **in context** of the **new thread**

- This is **important** when the **argument** supplied is a **pointer** to the **automatic variable**

---

```c++
void f(int i, std::string const& s);

void oops(int some_param){
    char buffer[1024]; // pointer to local vaiable buffer 
    sprintf(buffer, "%i", some_param);
    std::thread t(f, 3, buffer); // passes local buffer variable 
    t.detach();
}
```

- Significant chance the **function** `oops` *exits* **before** the **buffer** is converted to `std::string` 
  - Therefore **leads** to **undefined behaviour**.
- Just **cast** the `std::string` before **passing** to the **std::thread constructor**

```c++
void f(int i, std::string const& s);

void oops(int some_param){
    char buffer[1024];
    sprintf(buffer, "%i", some_param);
    std::thread t(f, 3, std::string(buffer)); // avoid dangling pointer, by casting
    t.detach();
}
```

- Issue here is **relying** on the **implicit conversion** of **pointer** to **buffer** > `std::string` 
- The `std::thread` constructor **copies** the **supplied values** as they are with **no conversion** to the **expected argument**.

---

- Object **may be copied** $\to$ preferred for **reference**
  - Could occur if the **thread** is *updating* some **data structure** that is **passed by reference**:

```C++
void update_data_for_widget(widget_id w,widget_data& data);
void oops_again(widget_id w)
{
     widget_data data;
     std::thread t(update_data_for_widget,w,data);
     display_status();
     t.join();
     process_widget_data(data);
}
```

- `update_data_for_widget` $\to$ expects the **second parameter** as **reference**
  - `std::thread` constructor **does not know this** $\to$ *blind copy*.
    - When called $\to$​ *references* its **internal copy** instead.
      - Thread ends $\to$ updates are **discarded** as **internal storage** is **destroyed**. 
- `process_widget_data` passed **unchanged data**
- **Wrap** the arguments that **must be references** in `std::ref` $\to$ changing the **thread invocation to**

```c++
std::thread t(update_data_for_widget, w, std::ref(data));
```

- Similar concept to `std::bind` as `std::thread` mechanism functions in **same mechanism** for there **definitions**.
- Can pass a **member function pointer** as the **function**
  - Given you supply **suitable object pointer** as **first argument**.

```c++
class X
{
public:
 void do_lengthy_work();
};
X my_x;
std::thread t(&X::do_lengthy_work,&my_x); /* invokes the method, based on the specified this value = my_x */

```

- Code invokes `my_x.do_lengthy_work()` on the **new thread**
  - The address of `my_x` supplied as **object pointer**
- The **third argument** is the **first argument** to the **member function** and so on.

---

- Alternative situation where arguments **can only be moved**
  - Data **within** an **object** is **transferred** to another, leaves the **original object** as **empty**
    - Example $\to$ `std::unique_ptr` $\to$ provide **automatic memory management** 

---

- Only **one** can be **given** at any **time**
  - If the **instance** is **destroyed** $\to$ the **pointed to object** is *deleted*
- *move constructor / move assignment* allow **ownership** of an **object** to be **transferred** between `std::unique_ptr` **instances**.
  - Leave **source object** with a `NULL` **pointer**.

---

- The **moving of values** enables objects **of this type** $\to$ **accepted** as **function parameters** or **return values**.
- If source is **temporary** $\to$ the **move** is **automatic**
- If source **named** $\to$ transfer is **requested** via `std::move` 

```c++
void process_big_object(std::unique_ptr<big_object>);

std::unique_ptr<big_object> p(new big_object);
p->prepare_data(42);
std::thread t(process_big_object,std::move(p));
```

- `std::move(p)` $\to$ transfer the **ownership** of `big_object` to **internal storage** 
  - Then *processes* into `process_big_object`.
- Many classes of **standard thread library** have *similar ownership semantics* as `std::unique_ptr` , `std::thread` one of which.
- `std:thread` $\to$ owns a **resource** and **not dynamic object** unlike `std::unique_ptr`
  - Every **instance** manages the **thread of execution**
    - The **ownership** can be **transferred** between **instances** as `std::thread` $\to$​ *movable* and **not copyable.**
      - Ensure a **single object** is *associated* with some **thread of execution** at *any one time*, giving **ability to transfer**.

---

### Transferring Ownership of thread

- Reasons of *transfer*:

  1. Write some **function** to **create a thread** for the **background** 
     - Passes the **ownership** to **calling function** rather than **waiting on completion**
  2. Create a **thread** and pass **ownership** in to **some function** that should **wait** for it **to complete


---

- Either one $\to$​ **must transfer ownership** from one place to the other.
- `std::thread` supports only **move operations** via `std::move` to achieve this.
  - Use this to **move ownership** of some **thread of execution** between **various instances** of `std::thread`


```c++
void some_function();
void some_other_function();

std::thread t1(some_function); // new thread started associated with t1
std::thread t2=std::move(t1);  // transfer ownership to t2 at construction of t2 via std::move

/* 
	t1 loses its associated thread of execution and now has none , thread running some function
	is associated with t2.
*/

/* 
	new thread started associated with temp std::thread object 
	
	does not call std::move as its a temporary object which moves automatically and 
	implicitly.
*/

t1=std::thread(some_other_function);

std::thread t3; // default constructed thread, no associated execution thread

t3=std::move(t2); // ownership of t2 transferred to t3 with std::move as t2 is named.
t1=std::move(t3); // transfer ownership of t3 to t1 , calls std::terminate, as you cannot just drop a thread by assigning some new value to std::thread object that manages it.
```

