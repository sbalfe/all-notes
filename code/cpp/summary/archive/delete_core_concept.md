# C++

##### Operator overloading

- Overloading assignment (=), subscript ([]), function call (()), or member selection (->) as Member Functions

  - ```cpp
    lass MyClass {
    public:
        // Overloading assignment operator
        MyClass& operator=(const MyClass& other) {
            // Copy assignment logic here
            return *this;
        }
    
        // Overloading subscript operator
        int& operator[](int index) {
            // Subscript logic here
            return someInternalArray[index];
        }
    
        // Overloading function call operator
        void operator()() {
            // Function call logic here
        }
    
        // Assume MyClass is a smart pointer type for overloading member selection
        SomeType* operator->() {
            // Member selection logic here
            return ptr;
        }
    
    private:
        int someInternalArray[10];
        SomeType* ptr;
    };
    ```

- Overloading a unary operator as a  member function

  - ```cpp
    class Number {
    public:
        // Overloading unary minus operator
        Number operator-() const {
            Number result;
            result.value = -value;
            return result;
        }
        int value;
    };
    
    ```

- Overloading a binary operator that modifies its left operand as member function

  - ```cpp
    class Counter {
    public:
        // Overloading operator+=
        Counter& operator+=(const Counter& other) {
            count += other.count;
            return *this;
        }
    
        int count = 0;
    };
    ```

- Overloading a binary operator that does not modify its left operand as a normal function or friend function:

  - ```cpp
    class Vector2D {
    public:
        Vector2D(double x, double y) : x(x), y(y) {}
    
        friend Vector2D operator+(const Vector2D& a, const Vector2D& b);
    
        double x, y;
    };
    
    Vector2D operator+(const Vector2D& a, const Vector2D& b) {
        return Vector2D(a.x + b.x, a.y + b.y);
    }
    ```

- [Basic rules and idioms for operator overloading](https://stackoverflow.com/questions/4421706/what-are-the-basic-rules-and-idioms-for-operator-overloading/4421729#4421729)

  - ![image-20231120234542059](images/image-20231120234542059.png)

##### Learncpp-misc

- Conditional operator:

  - ```cpp
    int x = 10, y = 5;
    int greater = (x > y) ? x : y; // If x is greater than y, assign x to greater, else assign y
    ```

- Operating incrementing and decrementing:

  - ![image-20231120214752148](images/image-20231120214752148.png)

  - Side effects can cause order of evaluation issues:

    - ```cpp
      #include <iostream>
      
      int add(int x, int y)
      {
          return x + y;
      }
      
      int main()
      {
          int x { 5 };
          int value{ add(x, ++x) }; // undefined behavior: is this 5 + 6, or 6 + 6?
          // It depends on what order your compiler evaluates the function arguments in
      
          std::cout << value << '\n'; // value could be 11 or 12, depending on how the above line evaluates!
      
          return 0;
      }
      ```

  


##### Misc Reminders

- `->` operator:

  - overloading this essentially means we return some internal object from which we access its underlying values

  - If a pointer is returned we call the underlying operator, if this further calls another `->` then we repeat until we reach the end.

   ```cpp
  struct A {
      std::string* operator->(){
          return new std::string{"A string"};
      }
  }
  
  struct B {
      A operator->(){
          return A();
      }
  }
  
  int main(){
      B b;
      b->size();
  }
   ```

#### I/O & Strings

- [short string optimisation]()

- Basic file reading
- [std::string_view](https://www.learncpp.com/cpp-tutorial/introduction-to-stdstring/)
  - does not own the object its looking at
  - does not allocate memory when assigned to a string literal so less memory intensive
  - prefer over `const std::string& `
  - **[learncpp]** - **std::string_view** provides read-only access to an  existing string (a C-style string literal, a std::string, or a char  array) without making a copy. A `std::string_view` that is viewing a string that has been destroyed is sometimes called a **dangling** view. When a `std::string` is modified, all views into that `std::string` are **invalidated**, meaning those views are now invalid. Using an invalidated view (other than to revalidate it) will produce undefined behavior.


```cpp
#include <fstream>
#include <iostream>
#include <cstdlib>

int main(){
    std::ofstream output;
    
    if (!output){
        std::cerr << "error when attempting to open a stream" << "\n";
        exit(1); /* exit from cstdlib */
    }
    
    int num[5] { 1,2,3,4,5 };
    
    for (int i = 0; i < 5; i++){
        output << num[i] << "\n";
    }
    
    output.close();
    
}
```

- String comparison

```cpp
#include <iostream>
template<typename T>
T max_t(T a, T b)
{
    return b < a ? "first string" : "second string"; // Note that z > a for string comparison
}
int main() {
    const std::string str1 { "b" };
    const std::string str2 { "a" };
    constexpr auto ret = max_t(str1, str2);
    std::cout << ret << "\n";
}
```

- Converting strings to integers - https://en.cppreference.com/w/cpp/string/basic_string/stol
- [Filestreams](https://en.cppreference.com/w/cpp/io/basic_fstream)
- [std::flush](https://en.cppreference.com/w/cpp/io/manip/flush)
- [istreambuf_iterator vs istream_iterator](https://stackoverflow.com/questions/1137575/inline-functions-vs-preprocessor-macros)
- Iterator default initial value

```cpp
#include <iostream>
#include <iterator>
#include <algorithm>
#include <sstream>
int main()
{
    std::istringstream stream("1 2 3 4 5");
    std::copy(
        std::istream_iterator<int>(stream),
        std::istream_iterator<int>(), /* default constructor just sets to end of the file.*/
        std::ostream_iterator<int>(std::cout, " ")
    );
}
```

##### Misc reminders

- [Checking an iterator against null](https://stackoverflow.com/questions/41352941/can-i-check-a-c-iterator-against-null)

- [std::fixed](https://www.cplusplus.com/reference/ios/fixed/)

- Vector standard implementation:

  - ![image-20231121003830668](images/image-20231121003830668.png)

- When floatfield is set to fixed, floating-point values are written using fixed-point notation: the value is represented with exactly as many digits in the decimal part as specified by the *precision field* ([precision](https://www.cplusplus.com/ios_base::precision)) and with no exponent part.

- **std::variant**

  - Alternative to runtime polymorphism

  ```cpp
  #include <iostream>
  
  struct make_string_functor {
      std::string operator()(const std::string& x) const {return x;}
      std::string operator()(int x){return std::to_string(x);}
  }
  
  int main(){
      const std::variant<int, std::string> v = "hello";
      
      std::cout << std::visit(make_string_functor(), v) << '\n';
      
      std::visit([](const auto&x){std::cout << x;}, v);
      std::cout << '\n';
  }
  ```

- **std::ranges::generate**

```cpp
for (auto& arr : datasets) {
		std::ranges::generate(arr, f);
	}

/* fills each value of the container with the f being called at each o*/
```

- **Remove specific item** from `std::vector`

```cpp
std::vector<int> items{ 1,2,3,4 };

/* remove the item at 4 by incrementing the start pointer */
items.erase(items.begin() + 3);
```

- **Float value between a and b**

```cpp
std::random_device rd;
std::default_random_engine gen(rd());
/* replace with a and b and make a function, also set option for a custom generator */
std::uniform_real_distribution<> distr(0, 1);

for (int n = 0; n < 5; ++n) {
	std::cout << std::setprecision(10) << distr(gen)<< "\n";
}
```

- `std::begin` vs `std::vector::begin` - https://stackoverflow.com/questions/26290316/difference-between-vectorbegin-and-stdbegin
  - begin is basically the generic form, it would call begin on the vector if called on it, however it would deal with edge cases.

- [Emplace back vs push back](https://stackoverflow.com/questions/4303513/push-back-vs-emplace-back)

### Modern-CPP-Cheatsheet

### C++11

##### C++ New language features

###### Move semantics

- Moving an object means to *transfer ownership* of some resource it manages to another object.
- The first benefit of move semantics is performance optimization. When an object is about to reach the end of its lifetime, either because it's a temporary or by explicitly calling `std::move`, a move is often a cheaper way to transfer resources. For example, moving a `std::vector` is just copying some pointers and internal state over to the new vector -- copying would involve having to copy every single contained element  in the vector, which is expensive and unnecessary if the old vector will soon be destroyed.
- Moves also make it possible for non-copyable types such as `std::unique_ptr` (**smart pointers**) to guarantee at the language level that there is only ever one instance of a resource being managed at a time, while being able to transfer an instance between scopes.
- See: **rvalue reference, special member functions for move semantics, std::move, std::forward and forwarding_references**

###### Rvalue references

- C++11 introduces a new reference termed the *rvalue reference*. An rvalue reference to `T`, which is a non-template type parameter (such as `int`, or a user-defined type), is created with the syntax `T&&`. Rvalue references only bind to rvalues.

  - Type deduction with lvalues and rvalues:

    ```cpp
    int x = 0; // `x` is an lvalue of type `int`
    int& xl = x; // `xl` is an lvalue of type `int&`
    int&& xr = x; // compiler error -- `x` is an lvalue
    int&& xr2 = 0; // `xr2` is an lvalue of type `int&&` -- binds to the rvalue temporary, `0`
    
    void f(int& x) {}
    void f(int&& x) {}
    
    f(x);  // calls f(int&)
    f(xl); // calls f(int&)
    f(3);  // calls f(int&&)
    f(std::move(x)); // calls f(int&&)
    
    f(xr2);           // calls f(int&)
    f(std::move(xr2)); // calls f(int&& x)
    ```

###### Forwarding references

- Also referred to as *universal references*. Created via the syntax with the syntax `T` where `T` is a template type parameter. Also created using `auto&&` (Which is conceptually the same rules as a `T` template typename parameter)

- This enables perfect forwarding. This is the ability to pass arguments while maintaining their *value category* (lvalues = lvalues and temporaries (rvalues = pr value or xvalue ) are forwarded as rvalues).

- Forwarding references allow a reference to bind to either an lvalue or  rvalue depending on the type. Forwarding references follow the rules of *reference collapsing*:

  - `T& &` becomes `T&`
  - `T& &&` becomes `T&`
  - `T&& &` becomes `T&`
  - `T&& &&` becomes `T&&`

- `auto` type deduction with lvalues and rvalues

  ```cpp
  int x = 0; // `x` is an lvalue of type `int`
  auto&& al = x; // `al` is an lvalue of type `int&` -- binds to the lvalue, `x`
  auto&& ar = 0; // `ar` is an lvalue of type `int&&` -- binds to the rvalue temporary, `0`
  ```

- Template type parameter deduction with lvalues and rvalue

  ```cpp
  // Since C++14 or later:
  void f(auto&& t) {
    // ...
  }
  
  // Since C++11 or later:
  template <typename T>
  void f(T&& t) {
    // ...
  }
  
  int x = 0;
  f(0); // T is int, deduces as f(int &&) => f(int&&)
  f(x); // T is int&, deduces as f(int& &&) => f(int&)
  
  int& y = x;
  f(y); // T is int&, deduces as f(int& &&) => f(int&)
  
  int&& z = 0; // NOTE: `z` is an lvalue with type `int&&`.
  f(z); // T is int&, deduces as f(int& &&) => f(int&)
  f(std::move(z)); // T is int, deduces as f(int &&) => f(int&&)
  ```

###### Variadic templates

- The `...` syntax creates a *parameter pack* or expands one. A template *parameter pack* is a template parameter that accepts zero or more template arguments  (non-types, types, or templates). A template with at least one parameter pack is called a *variadic template*.

  ```cpp
  template <typename... T>
  struct arity {
    constexpr static int value = sizeof...(T);
  };
  static_assert(arity<>::value == 0);
  static_assert(arity<char, short, int>::value == 3);
  ```

- An interesting use for this is creating an *initializer list* from a *parameter pack* in order to iterate over variadic function arguments.

  ```cpp
  template <typename First, typename... Args>
  auto sum(const First first, const Args... args) -> decltype(first) {
    const auto values = {first, args...};
    return std::accumulate(values.begin(), values.end(), First{0});
  }
  
  sum(1, 2, 3, 4, 5); // 15
  sum(1, 2, 3);       // 6
  sum(1.5, 2.0, 3.7); // 7.2
  ```

###### Initializer lists

- A lightweight array-like container of elements created using a "braced list" syntax. For example, `{ 1, 2, 3 }` creates a sequences of integers, that has type `std::initializer_list<int>`. Useful as a replacement to passing a vector of objects to a function.

  ```cpp
  int sum(const std::initializer_list<int>& list) {
    int total = 0;
    for (auto& e : list) {
      total += e;
    }
  
    return total;
  }
  
  auto list = {1, 2, 3};
  sum(list); // == 6
  sum({1, 2, 3}); // == 6
  sum({}); // == 0
  ```

###### Static assertions

- Assertions evaluated at compile time

  ```cpp
  constexpr int x = 0;
  constexpr int y = 1;
  static_assert(x == y, "x != y");
  ```

###### auto

- `auto`-typed variables are deduced by the compiler according to the type of their initializer.

  ```cpp
  auto a = 3.14; // double
  auto b = 1; // int
  auto& c = b; // int&
  auto d = { 0 }; // std::initializer_list<int>
  auto&& e = 1; // int&&
  auto&& f = b; // int&
  auto g = new auto(123); // int*
  const auto h = 1; // const int
  auto i = 1, j = 2, k = 3; // int, int, int
  auto l = 1, m = true, n = 1.61; // error -- `l` deduced to be int, `m` is bool
  auto o; // error -- `o` requires initializer
  ```

- Extremely useful for readability, especially for complicated types

  ```cpp
  std::vector<int> v = ...;
  std::vector<int>::const_iterator cit = v.cbegin();
  // vs.
  auto cit = v.cbegin();
  ```

- Functions can deduce the return type using `auto`. In C++11, a return type must be specified either explicitly, or using `decltype` like so:

  ```CPP
  template <typename X, typename Y>
  auto add(X x, Y y) -> decltype(x + y) {
    return x + y;
  }
  add(1, 2); // == 3
  add(1, 2.0); // == 3.0
  add(1.5, 1.5); // == 3.0
  ```

- The trailing return type in the above example is the *declared type* (see section on **decltype**) of the expression `x + y`. For example, if `x` is an integer and `y` is a double, `decltype(x + y)` is a double. Therefore, the above function will deduce the type depending on what type the expression `x + y` yields. Notice that the trailing return type has access to its parameters, and `this` when appropriate.

###### Lambda expressions

##### C++11 New Library features

###### std::move

- `std::move` indicates that the object passed to it may have  its resources transferred. Using objects that have been moved from  should be used with care, as they can be left in an unspecified state.

- Definition:

  ```cpp
  // Simply casts the value moved in to a r value reference via removing the reference (if present) then returning that
  template <typename T> typename remove_reference<T>::type&& move(T&& arg) {
    return static_cast<typename remove_reference<T>::type&&>(arg);
  }
  ```

- Transferring `std::unique_ptr` 

  ```cpp
  std::unique_ptr<int> p1 {new int{0}};  // in practice, use std::make_unique
  std::unique_ptr<int> p2 = p1; // error -- cannot copy unique pointers
  std::unique_ptr<int> p3 = std::move(p1); // move `p1` into `p3`
                                           // now unsafe to dereference object held by `p1`
  ```

###### std::forward

- Returns the arguments passed to it while maintaining their *value category* and *cv-qualifiers*. 

- Excellent for generic code and factories. Used in conjunction with *forwarding references*.

- Definition:

  ```cpp
  // This is a clever function that takes various args and applies reference collapsing depending if 
  // an l-value or r-value has been passed in.
  template <typename T> T&& forward(typename remove_reference<T>::type& arg) {
    return static_cast<T&&>(arg);
  }
  ```

- An example of a function `wrapper` which just forwards other `A` objects to a new `A` object's copy or move constructor:

  ```cpp
  struct A {
    A() = default;
    A(const A& o) { std::cout << "copied" << std::endl; }
    A(A&& o) { std::cout << "moved" << std::endl; }
  };
  
  template <typename T>
  A wrapper(T&& arg) {
    return A{std::forward<T>(arg)};
  }
  
  wrapper(A{}); // moved
  A a;
  wrapper(a); // copied
  wrapper(std::move(a)); // moved
  ```

###### std::thread

- The `std::thread` library provides a standard way to control  threads, such as spawning and killing them. In the example below,  multiple threads are spawned to do different calculations and then the  program waits for all of them to finish.

  ```cpp
  void foo(bool clause) { /* do something... */ }
  
  std::vector<std::thread> threadsVector;
  threadsVector.emplace_back([]() {
    // Lambda function that will be invoked
  });
  threadsVector.emplace_back(foo, true);  // thread will run foo(true)
  for (auto& thread : threadsVector) {
    thread.join(); // Wait for threads to finish
  }
  ```

###### std::to_string

- Converts a numeric argument to `std::string`

  ```cpp
  std::to_string(1.2); // == "1.2"
  std::to_string(123); // == "123"
  ```

###### Type traits

- Type traits defines a compile-time template-based interface to query or modify the properties of types.

  ```cpp
  static_assert(std::is_integral<int>::value);
  static_assert(std::is_same<int, int>::value);
  
  // https://en.cppreference.com/w/cpp/types/conditional
  static_assert(std::is_same<std::conditional<true, int, double>::type, int>::value);
  ```

###### Smart pointers

- C++11 introduces new smart pointers: `std::unique_ptr`, `std::shared_ptr`, `std::weak_ptr`. `std::auto_ptr` now becomes deprecated and then eventually removed in C++17.

- `std::unique_ptr` is a non-copyable, movable pointer that manages its own heap-allocated memory. **Note: Prefer using the `std::make_X` helper functions as opposed to using constructors**

  ```cpp
  std::unique_ptr<Foo> p1 { new Foo{} };  // `p1` owns `Foo`
  if (p1) {
    p1->bar();
  }
  
  {
    std::unique_ptr<Foo> p2 {std::move(p1)};  // Now `p2` owns `Foo`
    f(*p2);
  
    p1 = std::move(p2);  // Ownership returns to `p1` -- `p2` gets destroyed
  }
  
  if (p1) {
    p1->bar();
  }
  // `Foo` instance is destroyed when `p1` goes out of scope
  ```

- A `std::shared_ptr` is a smart pointer that manages a resource that is shared across multiple owners. A shared pointer holds a *control block* which has a few components such as the managed object and a reference  counter. All control block access is thread-safe, however, manipulating  the managed object itself is *not* thread-safe.

  ```cpp
  void foo(std::shared_ptr<T> t) {
    // Do something with `t`...
  }
  
  void bar(std::shared_ptr<T> t) {
    // Do something with `t`...
  }
  
  void baz(std::shared_ptr<T> t) {
    // Do something with `t`...
  }
  
  std::shared_ptr<T> p1 {new T{}};
  // Perhaps these take place in another threads?
  foo(p1);
  bar(p1);
  baz(p1);
  ```

###### std::chrono

- The chrono library contains a set of utility functions and types that deal with *durations*, *clocks*, and *time points*. One use case of this library is benchmarking code:

  ```cpp
  std::chrono::time_point<std::chrono::steady_clock> start, end;
  start = std::chrono::steady_clock::now();
  // Some computations...
  end = std::chrono::steady_clock::now();
  
  std::chrono::duration<double> elapsed_seconds = end - start;
  double t = elapsed_seconds.count(); // t number of seconds, represented as a `double`
  ```

###### Tuples

- Tuples are a fixed-size collection of heterogeneous (could be homogenous technically) values. Access the elements of a `std::tuple` by unpacking using [`std::tie`](https://github.com/AnthonyCalandra/modern-cpp-features#stdtie), or using `std::get`. 

- A generalisation of `std::pair` to more lengths.

  ```cpp
  // `playerProfile` has type `std::tuple<int, const char*, const char*>`.
  auto playerProfile = std::make_tuple(51, "Frans Nielsen", "NYI");
  std::get<0>(playerProfile); // 51
  std::get<1>(playerProfile); // "Frans Nielsen"
  std::get<2>(playerProfile); // "NYI"
  ```

###### std::tie

- Creates a tuple of lvalue references. Useful for unpacking `std::pair` and `std::tuple` objects. Use `std::ignore` as a placeholder for ignored values. In C++17, **structured bindings** should be used instead.

  ```cpp
  // With tuples...
  std::string playerName;
  std::tie(std::ignore, playerName, std::ignore) = std::make_tuple(91, "John Tavares", "NYI");
  
  // With pairs...
  std::string yes, no;
  std::tie(yes, no) = std::make_pair("yes", "no");
  ```

###### std::array

- `std::array` is a container built on top of a C-style array. Supports common container operations such as sorting.

  ```cpp
  std::array<int, 3> a = {2, 1, 3};
  std::sort(a.begin(), a.end()); // a == { 1, 2, 3 }
  for (int& x : a) x *= 2; // a == { 2, 4, 6 }
  ```

###### Unordered containers

- These containers maintain *average constant-time complexity* for search,  insert, and remove operations. In order to achieve constant-time complexity, sacrifices order for speed by *hashing elements* into buckets. There are four unordered containers:
  - `unordered_set`
  - `unordered_multiset`
  - `unordered_map`
  - `unordered_multimap`

###### std::make_shared

- `std::make_shared` is the recommended way to create instances of `std::shared_ptr` for the following reasons:

  - Avoid having to use the `new` operator.

  - Prevents code repetition when specifying the underlying type the pointer shall hold.

  - It provides *exception-safety*. Suppose we were calling a function `foo` like so:

    ```cpp
    foo(std::shared_ptr<T>{new T{}}, function_that_throws(), std::shared_ptr<T>{new T{}});
    ```

    - The compiler is free to call `new T{}`, then `function_that_throws()`, and so on... Since we have allocated data on the heap in the first construction of a `T`, we have introduced a leak here. With `std::make_shared`, we are given exception-safety:

    ```cpp
    foo(std::make_shared<T>(), function_that_throws(), std::make_shared<T>());
    ```

  - Prevents having to do two allocations. When calling `std::shared_ptr{ new T{} }`, we have to allocate memory for `T`, then in the shared pointer we have to allocate memory for the control block within the pointer.

###### std::ref

- `std::ref(val)` is used to create object of type `std::reference_wrapper` that holds reference of val. Used in cases when usual reference passing using `&` does not compile or `&` is dropped due to type deduction. `std::cref` is similar but created reference wrapper holds a const reference to val.

  ```cpp
  // create a container to store reference of objects.
  auto val = 99;
  auto _ref = std::ref(val);
  _ref++;
  auto _cref = std::cref(val);
  //_cref++; does not compile
  std::vector<std::reference_wrapper<int>>vec; // vector<int&>vec does not compile
  vec.push_back(_ref); // vec.push_back(&i) does not compile
  cout << val << endl; // prints 100
  cout << vec[0] << endl; // prints 100
  cout << _cref; // prints 100
  ```

###### Memory model

- C++11 introduces a memory model for C++, which means library support for threading and atomic operations. Some of these operations include (but aren't limited to) atomic loads/stores, compare-and-swap, atomic flags, promises, futures, locks, and condition variables.

###### std::async

- `std::async` runs the given function either *asynchronously* or *lazily-evaluated,* then returns a `std::future` which holds the result of that function call.

- The first parameter is the policy which could be:

  1. `std::launch::async | std::launch::deferred` It is up to the implementation whether to perform asynchronous execution or lazy evaluation.

  2. `std::launch::async` Run the callable object on a new thread.

  3. `std::launch::deferred` Perform lazy evaluation on the current thread.

  ```cpp
  int foo() {
    /* Do something here, then return the result. */
    return 1000;
  }
  
  auto handle = std::async(std::launch::async, foo);  // create an async task
  auto result = handle.get();  // wait for the result
  ```

###### std::begin and std::end

- `std::begin` and `std::end` free functions were  added to return begin and end iterators of a container generically.  These functions also work with raw arrays which do not have `begin` and `end` member functions.

  ```cpp
  template <typename T>
  int CountTwos(const T& container) {
    return std::count_if(std::begin(container), std::end(container), [](int item) {
      return item == 2;
    });
  }
  
  std::vector<int> vec = {2, 2, 43, 435, 4543, 534};
  int arr[8] = {2, 43, 45, 435, 32, 32, 32, 32};
  auto a = CountTwos(vec); // 2
  auto b = CountTwos(arr);  // 1
  ```

- 

#### **C++14**

#### C++17

- **Structured bindings**
- https://stackoverflow.com/questions/62871344/using-structured-binding-declaration-in-range-based-for-loop


  - https://en.cppreference.com/w/cpp/language/structured_binding

- **[[nodiscard]]**

  - [Why not use no discard everywhere](https://softwareengineering.stackexchange.com/questions/363169/whats-the-reason-for-not-using-c17s-nodiscard-almost-everywhere-in-new-c)

#### C++20

##### C++ 20 New Library Features

- [Spaceship operator](https://devblogs.microsoft.com/cppblog/simplify-your-code-with-rocket-science-c20s-spaceship-operator/)

#### C++23

### Templates 

##### cppcon**Concepts**

##### Misc reminders

- Type traits primer in C++ - https://www.internalpointers.com/post/quick-primer-type-traits-modern-cpp

- Type traits - `is_standard_layout` - https://en.cppreference.com/w/cpp/types/is_standard_layout

- [Explicit template initialisation use cases](https://stackoverflow.com/questions/2351148/explicit-template-instantiation-when-is-it-used)

- [Character traits (char_traits)](https://stackoverflow.com/questions/2351148/explicit-template-instantiation-when-is-it-used)

- [Inline keyword for templates](https://stackoverflow.com/questions/10535667/does-it-make-any-sense-to-use-inline-keyword-with-templates)

- [Tag dispatching](https://www.fluentcpp.com/2018/04/27/tag-dispatching/)

- **Passing types to templates**

  - ![image-20231119184341988](images/image-20231119184341988.png)

  - ```cpp
    template<template<typename> class F,class T>
    struct Crapdaptor
    {
        bool operator()( const std::unique_ptr<T>& pl,const std::unique_ptr<T>& pr ) const
        {
            return F<T>{}( *pl,*pr );
        }
    };
    ```


- `typename` keyword.
  - [cppref typename](https://en.cppreference.com/w/cpp/keyword/typename)
  - ![image-20231120233610133](images/image-20231120233610133.png)

**Abbreviated function templates**

```cpp
auto max(auto x, auto y)
{
    return (x > y) ? x : y;
}

/* equivalent too*/

template <typename T, typename U>
auto max(T x, U y)
{
    return (x > y) ? x : y;
}
```

**Simple Variadic Template example**

```cpp
/* used for when the parameter pack is empty.*/
void print() {
}

/* takes in generic type and generic number of items*/
template<typename T, typename... Types>

/* takes in the first item, then the second item is the parameter pack (...) denoted args*/
void print(T firstarg, Types... args) {
	std::cout << firstarg << "\n";
	/* pass the parameter pack recurisvely to repeat this process to operate on each value*/
	print(args...);
}

int main(){
   /* generic printing is a common application of template metaprogramming*/
   print(1, "fuck", false) // 1, "fuck" ,0 
}
```

**User defined deduction guides**

- ```cpp
  template <typename T, typename U>
  struct pair
  {
      T first{};
      U second{};
  };
  
  /* this tells us that when we initialise a pair with some values T,U the type deduced should be pair<T,U>*/
  template <typename T, typename U>
  pair(T, U) -> pair<T, U>;
  
  int main()
  {
      pair<int, int> p1{ 1, 2 }; 
      pair p2{ 1, 2 };     
  
      return 0;
  }
  ```

**void_t explanation for SFINAE**

- https://riptutorial.com/cplusplus/example/3778/void-t#:~:text=C%2B%2B11,facilitate%20writing%20of%20type%20traits

```CPP
template <class T, class=void>
struct has_foo : std::false_type {};

// Passing a type T without a foo function defined causes a substitution error and therefore falls back on the false type.
template <class T>
struct has_foo<T, void_t<decltype(std::declval<T&>().foo())>> : std::true_type {};
```
