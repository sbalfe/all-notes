# Delivering coroutines

### Function vs Coroutine

![image-20231124222506901](./images/image-20231124222506901.png)

- Coroutines can *suspend* and *resume* at various points. 
- The Coroutine itself decides when to suspend itself.

### What are Coroutines

- There are two forms of coroutines
  - Stackfull.
  - Stackless (The C++ form)
- Stackless means that the data of a coroutine, its *coroutine frame*, is stored on the heap.
- We are talking about *cooperative multitasking* [wiki](https://en.wikipedia.org/wiki/Cooperative_multitasking) when using coroutines.
- Coroutines can simplify the code
  - We can replace function pointers (callbacks) with coroutines.
  - Parsers are much more readable with coroutines.

### Interacting with a Coroutine

- Coroutines as mentioned may be paused and resumed
  - `co_yield` or `co_await` will *pause* a coroutine.
  - `co_return` will *end* a coroutine.

![image-20231124233053519](./images/image-20231124233053519.png)

- `co_yield` - is where the coroutine becomes suspended and therefore we can resume it later.
- `co_return` - coroutine is finished therefore we should not supposed to resume this coroutine (UB).
- `co_await` - this is for input, but we can only pass parameters at the point it starts. What if we need to feed some data into it after this, such as *calling again*. We use co_await to suspend execution and wait on another async operation i.e another coroutine.

### Elements of a Coroutine

- **Components of a C++ coroutine.**
  - A *Wrapper type*.
    - This is the type of the coroutine function's prototype. 
    - We can control the the coroutine from the outside. For example, resuming the coroutine or getting data into or from the coroutine by storing a handle to the coroutine in the wrapper type.
  - The compiler looks for a type with the exact name `promise_type` inside the return type of the coroutine (the wrapper type). This is the control from the inside. The type could be:
    - A Type alias
    - Typedef
    - Declared directly in the coroutine wrapper type.
  - An awaitable type that comes into play once we use `co_await`
  - Typically use another part, an iterator.
- A coroutine in C++ is a finite state machine (FSM) that can be controlled via the `promise_type`.
- The actual coroutine function that uses `co_yield` , `co_await` or `co_return` for communication with the world out side.

### Coroutine chat

```cpp
Chat Fun() // Wrapper type Chat containing the promise type
{
    co_yield "Hello!\n"s; // Calls promise_type.yield_value
    std::cout << co_await std::string{}; // Calls promise_type.await_transform
    co_return "Here!\n"s; // Calls promise_type.return_value
}

void Use()
{
    Chat chat = Fun(); // Creation of the coroutine
    std::cout << chat.listen(); // Trigger the machine
    chat.answer("Where are you?\n"s); // Send data into the coroutine
    std::cout << chat.listen(); // Wait for more data from the coroutine
}
```

