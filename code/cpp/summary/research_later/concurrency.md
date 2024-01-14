# Concurrency in Action

### Managing threads

- All C++ programs have at least one thread that runs `main()`. This is started by the C++ runtime.
- You can launch threads alongside the main thread via `std::thread` and then wait for them to finish.
- Launching threads boils down to construction of the `std::thread` object.

```cpp
void do_some_work();
std::thread my_thread(do_some_work);
```

- `std::thread` works with any *callable* type, such as:
  - Lambda expression
  - Functor

```cpp
// Functor
struct Callable {
    void operator()() const {
        DoSomething();
    }
}
Callable c;
std::thread my_thread {c}; // Copied into the storage of this thread and invoked within it.

//Lambda
std::thread([](){
    DoSomething();
});
```

- We must `join` or`detach` the threads after starting. If we do not then once the program exits and the thread is still running , `std::terminate` would be called.

- We must also consider **dangling references** when a thread object `std::thread` is destroyed. This is because its **undefined behaviour** to access an object after its destruction. An example would be passing a function local variable with automatic duration to `std::thread` such as referencing it as a member variable of the functor. When the program ends the reference will be dangling as `some_local_state` in the example below is destroyed.

```cpp
struct func
{
    int& i; // reference variable will dangle when oops exists.
    func(int& i_):i(i_) {}
    void operator()()
    {
        for (unsigned j = 0; j < 1000000; ++j)
        {
            do_something(i);
        }
    }
};

void oops()
{
    int some_local_state = 0;
    func my_func(some_local_state);
    std::thread my_thread(my_func);
    my_thread.detach(); // thread runs in the background, and will exit in its own time.
}

```

- We can fix this by just copying the state to the thread struct instead or just `join` the thread.

- `join` cleans up any storage associated with the thread.
- Make sure that `join` is not skipped as via some condition triggered / exception being thrown. 

```cpp
void f(){
    int some_local_state=0;
    func my_func(some_local_state);
    std::thread t(my_func);
    try{
        do_something_in_current_thread();
    }
    catch(...){
        t.join(); // ensure you join in the exception
        throw;
    }
    t.join();
}
int main(){
    f();
}
```

- Prefer to use **RAII** to ensure a join of a thread is met. where the destructor calls `join` on the thread its wrapping. Use `std::jthread` in C++20. If this is not required then use `detach`.
- When we `detach` a thread its called a **daemon thread**. These typically perform a long running background task essential for the operation of some programs such as file system monitoring. 
- We may `detach` a thread if another mechanism can determine when the thread should execute or when the thread is used as a "fire and forget task".
- When you call `detach`, the `std::thread` returns `false` for `joinable`.