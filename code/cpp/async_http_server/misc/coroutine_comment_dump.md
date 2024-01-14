```cpp
#include <coroutine>
#include <fmt/format.h>

struct ReturnType {

    // Slightly related to std::promise where results are placed for futures to get.

    // determines what happens at various bits of a coroutines lifetime
    struct promise_type {

        // This function is always the same as the coroutine function itself.
        // the compiler implicitly calls this get_return_object,  so its our
        // promises type to construct the return object which is the handed back to the caller.
        // compiler stores this somewhere else, when we reach a suspension point / end of control flow, this object is returned.
        // to the caller.
        ReturnType get_return_object() {return {};}


        // customisation point to execute code when the coroutine just starts executing.

        // suspend always - pause execution at this point and hand control flow back to the caller
        // suspend never - just keep executing in the coroutine.
        std::suspend_never initial_suspend() {return {};}

        // what happens when we reach the co_return statement, define customisation here.
        // we can use return_value if we acc co_return a value.
        void return_void(){fmt::print("void\n");}

        // what happens if we have an exception and exit our coroutine
        void unhandled_exception(){}

        // customisation point to execute code when the coroutine is just about to terminate execution fully via
        // an exception or control flow ended
        std::suspend_always final_suspend() noexcept {return {};}
    };
};

ReturnType func(){
    fmt::print("Hello from coroutine!\n");
    co_return;
}

int main(){
    func();
}
```

```CPP
#include <coroutine>
#include <fmt/format.h>

struct ReturnType {

    struct promise_type;

    // the handle has a couple of key functions
    // resume() will resume a suspended coroutine.
    // destroy() will wipe the coroutine state, terminating as if it has reached the end of its flow
    // promise() convert from the handle to the promise object
    // from_promise() vice versa
    using coroutine_handle = std::coroutine_handle<promise_type>;

    struct SuspendAlways {
        // determines if we go into suspension or continue executing
        // async operation may be ready or may not be ready.
        // it returns a boolean to say if we should wait or not, if its false always we have
        // implemneted std::suspend_always awaitable essentially, likewise with always true for suspend never.
        bool await_ready(){return false;}

        // Execution function that we run shortly before the coroutine goes to suspension mode.
        // we obtain a handle to the coroutine, which gives us access to the coroutines frame (heap usually)
        // Type erased template that can point to any coroutine.
        // typically we have a handle of the promise type, so it is now restricted to pointing to frames that use our promise type.
        void await_suspend(coroutine_handle){}

        // executed right before the coroutine wakes up after the caller resumes us.
        void await_resume(){}

    };

    // can specialise the std::coroutine_traits<ReturnType, ...>
    // if we wish to not nest our type here.
    struct promise_type {

        /*              SETUP                 */
        // return type determines what the promise type will be.
        // promise_type(T...) // arguments passed here will be the same arguments on our function signatur or we just construct
        // the type directly.

        // pass return object back to caller.
        ReturnType get_return_object() {return {};}
        // coroutine starts, function body executing.
        std::suspend_never initial_suspend() {return {};}

        /*              SHUTDOWN                 */
        void return_void(){}
        void unhandled_exception(){}
        std::suspend_always final_suspend() noexcept {return {};}
    };
};

ReturnType func(){
    fmt::print("Hello from coroutine!\n");
    co_return;
}

int main(){
    func();
}
```

```cpp
#include <coroutine>
#include <fmt/format.h>

struct ReturnType {
    struct promise_type;
    using coroutine_handle = std::coroutine_handle<promise_type>;
    coroutine_handle coro_handle;

    ReturnType(coroutine_handle h): coro_handle(h) {}
    void resume(){
        coro_handle.resume();
    }
    
    struct promise_type {
        ReturnType get_return_object() {
            return ReturnType{std::coroutine_handle<promise_type>::from_promise(*this)};
        }
        std::suspend_always initial_suspend() {return {};}
        void return_void(){}
        void unhandled_exception(){}
        std::suspend_always final_suspend() noexcept {return {};}
    };
};

ReturnType hello_coroutine(){
    fmt::print("Hello from coroutine!\n");
    co_return;
}

int main(){
    ReturnType c = hello_coroutine();
    c.resume();
}
```

```cpp
#include <coroutine>
#include <fmt/format.h>

struct Coroutine {
    struct promise_type;
    using coroutine_handle = std::coroutine_handle<promise_type>;
    coroutine_handle coro_handle;

    Coroutine(coroutine_handle h): coro_handle(h) {}
    void resume(){
        coro_handle.resume();
    }

    struct TheAnswer {
        int value_;
        TheAnswer(int v): value_ {v}{}
        bool await_ready(){return false;}
        void await_suspend(coroutine_handle h){
            // our coroutine can attach the promise via the handle passed when it has await suspended.
            // we can store the data in our promise here, which the caller can access via the return type which
            // also has a handle to the promise from the object returned.
            h.promise().value = value_;
        }
        void await_resume(){}
    };

    int GetAnswer(){
        return coro_handle.promise().value;
    }

    ~Coroutine(){
        coro_handle.destroy();
        fmt::print("destroy coroutine handle\n");
    }

    // Promise is the central intersection point between awaitables (coroutine) and return type (caller).
    struct promise_type {
        int value;

        Coroutine get_return_object() {
            return Coroutine{std::coroutine_handle<promise_type>::from_promise(*this)};
        }
        std::suspend_always initial_suspend() {return {};}
        void return_void(){}
        void unhandled_exception(){}
        std::suspend_always final_suspend() noexcept {return {};}
    };
};

Coroutine f1(){
    co_await Coroutine::TheAnswer {42};
}

int main(){
    Coroutine c = f1();
    fmt::print("The answer is {}\n", c.GetAnswer());
}
```

https://www.youtube.com/watch?v=J7fYddslH0Q

```cpp
#include <coroutine>
#include <fmt/format.h>

struct Coroutine {
    struct promise_type;
    using coroutine_handle = std::coroutine_handle<promise_type>;
    coroutine_handle coro_handle;

    Coroutine(coroutine_handle h): coro_handle(h) {}
    void resume(){
        coro_handle.resume();
    }
    struct TheAnswer {
        int value_;
        explicit TheAnswer(int v): value_ {v}{}
        // we are not ready, we must suspend
        bool await_ready(){
            fmt::print("await ready\n");
            return false;
        }

        // we are ready suspend
        void await_suspend(std::coroutine_handle<promise_type> h){
            fmt::print("await suspend\n");
            h.promise().value = value_;
        }

        // we have been resumed
        void await_resume(){
            fmt::print("resume awaitable\n");
        }

    };
    int GetAnswer(){
        return coro_handle.promise().value;
    }

    ~Coroutine(){
        coro_handle.destroy();
        fmt::print("destroy coroutine handle\n");
    }

    struct promise_type {
        int value;
        Coroutine get_return_object() {
            fmt::print("getting our return object\n");
            return Coroutine{coroutine_handle::from_promise(*this)};
        }

        // these are awaitable type returned.
        std::suspend_always initial_suspend() {
            fmt::print("initial suspend\n");
            return {};
        }
        void unhandled_exception(){}

        // only called with co_return or exceptions
        std::suspend_always final_suspend() noexcept {
            fmt::print("final suspend\n");
            return {};
        }
    };
};

Coroutine f1(){
    fmt::print("Inside f1() awaiting the answer\n");
    co_await Coroutine::TheAnswer{42};
    fmt::print("after co_await\n");
}

int main(){
    fmt::print("Starting our coroutine initialization\n");
    Coroutine c = f1();
    fmt::print("obtained coroutine object, resuming\n");
    c.resume();
    fmt::print("The answer is {}\n", c.GetAnswer());
    c.resume();
}
```

