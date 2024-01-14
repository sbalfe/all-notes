# C++

> Things to Look into more
>
> - Review POD types, class layouts as defined in the [Class section.](#Classes, OOP, ...)
> - Review how type traits are implemented for a variety of examples. Notably review the Cppcon video.
> - Review The containers in the standard library, from a performance and use case perspective ==IMPORTANT==
>   - Ranges, iterators, algorithms, containers ...
> - Understand C++20 modules ==IMPORTANT==
> - Review The standard library as a whole looking at useful features ==IMPORTANT== 
> - Understand vector and string class under the hood - create your own maybe.
> - Understand Placement new
> - Understand bit manipulation from learncpp or elsewhere and why its useful ==IMPORTANT==.
> - Start collating a better understanding of template metaprogramming. ==IMPORTANT==
>   - Concepts, CRTP
>   - https://www.vishalchovatiya.com/variadic-template-cpp-implementing-unsophisticated-tuple/
> - Review & summarise auto rules. ==IMPORTANT==
> - Templates ... operator.
> - Clean up typing section ==IMPORTANT== 
> - Merge learncpp stuff on log slide into here - **not important but good**, notable topics to review
>   - Bit manipulation
> - Cppcon
>   - [Memory Allocators](https://www.youtube.com/watch?v=nZNd5FjSquk&t=538s)
>   - [Type Erasure](https://www.youtube.com/watch?v=tbUCHifyT24)
>   - [What is the C++ ABI](https://www.youtube.com/watch?v=DZ93lP1I7wU)
>   - [Important C++ Optimisations](https://www.youtube.com/watch?v=qCjEN5XRzHc)
>   - [Back 2 Basics Templates watch both parts](https://www.youtube.com/watch?v=XN319NYEOcE)
>   - [More init bullshit (useful maybe)](https://www.youtube.com/watch?v=7DTlWPgX6zs)
>   - [Rust vs C++ explained via static analysis](https://www.youtube.com/watch?v=_pQGRr4P16w)
> - Other
>   - [Good channel](https://www.youtube.com/@CodeForYourself)
>     - [Statics in modern C++](https://www.youtube.com/watch?v=7cpPQunjv4s)

### General C++

- [Runtime vs Compile time](https://stackoverflow.com/questions/846103/runtime-vs-compile-time)
- The main function implicitly returns `0`.
- C++ Escape characters:
  - ![image-20231120204941344](images/image-20231120204941344.png)

### Move Semantics

```cpp
std::string func1(const std::string& str){ /* implicit &memory_location if being assigned passed in as a param*/
    std::string tmp{str};
    return tmp; /* if func1() is being stored then RVO */
}

void wrap1(const std::string&){
    func1(arg);
}

template<typename T>
std::string func2(T&& arg){
    std::string tmp{std::forward<T>(arg)};
    return tmp;
}

template<typename T>
void wrap2(T&& arg){
    func2(std::forward<T>(arg)); // forward move semantics to another R value reference function so library makers speed up by distinguishing what can be copied and moved they can avoid spurios copies.
}

int main(){
    /* temp implictly is copied into a std::string temporary to bind to the const std::string&
    	copied again when constructed in func1
    */
    std::string data = "value";
    wrap1("temp");
    
    /* temp forwarded to the construction in func1 keeping it a temp so it only gets copied when
    	the std::string is being made
    */
    wrap2("temp");
}

```

### Initialisation

> Initialisation reference - https://en.cppreference.com/w/cpp/language/initialization

##### cpp-core-guidelines

- [C.49 Prefer initialization to assignment in constructors](C.49: Prefer initialization to assignment in constructors)

  - ```cpp
    class A {   // Good
        string s1;
    public:
        A(czstring p) : s1{p} { }    // GOOD: directly construct (and the C-string is explicitly named)
        // ...
    };
    ```

- [ES.23 Prefer the `{}` syntax for initialisation](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#es23-prefer-the--initializer-syntax)

  - use `=` for 

    - Built in types

    - Certain there are no narrowing conversions

    - With `auto ` only use `=`.

      - ```CPP
        auto x1 {1, 2, 3}; // error: not a single element
        auto x2 = {1, 2, 3}; // x2 is std::initializer_list<int>
        auto x3 {3}; // x3 is int
        auto x4 {3.0}; // x4 is double
        ```

##### Summary

- [Init in C++ is bonkers](http://mikelui.io/2019/01/03/seriously-bonkers.html)
- [Another init reference](https://accu.org/journals/overload/25/139/brand_2379/)

<img src="images/image-20231120230338039.png" alt="image-20231120230338039" style="zoom:67%;" />

##### Learncpp-misc

- [struct aggregate initialization](https://www.learncpp.com/cpp-tutorial/struct-aggregate-initialization/)
- [default member initialization](https://www.learncpp.com/cpp-tutorial/default-member-initialization/)

##### Copy & Move Elision

> cpp con talk - https://www.youtube.com/watch?v=IZbL-RGr_mk
>
> [What is copy elision and RVO](https://stackoverflow.com/questions/12953127/what-are-copy-elision-and-return-value-optimization/12953145#12953145)

- Compiler may generate some code. The compiler has the ability to elide copies that are not actually required. 
- Returning a value defined in the calling function stack frame is not defined but rather moved directly into the address.

<img src="images/image-20231118184218193.png" alt="image-20231118184218193" style="zoom:80%;" />

```cpp
std::string f(){
    std::string a{"A"}; // not created on the stack frame as shown above
    
    int b{23};
    
    return a; // "A" is returned here, placed directly into the memory location of x
}

void g(){
    std::string x{f()}; // f() takes &x as argument from where a is directly placed into by compiler optimsaition.
}
```

- Another image from the discord, notably stating **NRVO (named return value optimisation)**:

  - ![image-20231119201741152](images/image-20231119201741152.png)

  - both are the same elided result which skips the copy or move step into the returned value address passed in under the hood 

- **Summary of copy elision**

  - [Guaranteed copy elision](https://stackoverflow.com/questions/38043319/how-does-guaranteed-copy-elision-work) - modern.

    - [more details](https://www.open-std.org/jtc1/sc22/wg21/docs/papers/2015/p0135r0.html)
    - C++17 makes a pr value materialized only when required, then its constructed directly into the storage of its final destination. This means it may look like the syntax is copying and moving but no copy or move is actually performed. This is not really copy elision per se, its built into the language construct itself now, we dont need to define move or copy constructors like before.
	
  - RVO
  
      - NRVO (named return value optimisation) - This is where a value named in the function that has automatic duration is returned and the copy / move is elided.
  
        > in a [return statement](https://en.cppreference.com/w/cpp/language/return), when the operand is the name of a non-volatile object with automatic  storage duration, which isn't a function parameter or a catch clause  parameter, and which is of the same class type (ignoring [cv-qualification](https://en.cppreference.com/w/cpp/language/cv)) as the function return type. This variant of copy elision is known as NRVO, "named return value optimization."
  
      - URVO (unnamed return value optimisation) - This is the same as NRVO apart from the fact there was no name i.e there was no original 
  
        ```cpp
        struct Type {
        	Type() : x{ 0 } { std::print("default constructor"); }
        	Type(int val) : x{ val } { std::print("int constructor\n");}
        	Type(const Type& obj) : x{ obj.x } { std::print("copy construct\n"); }
        	Type(Type&& obj) noexcept : x{obj.x} { std::print("move construct\n"); }
        	int x;
        };
        
        Type NVRO() {
        	Type inner_var{ 2 };
        	return inner_var;
        }
        
        Type UVRO() {
        	return Type{ 2 };
        	//return { 2 }; same as the above unnamed
        }
        
        int main() {
        	Type t{ UVRO() };
        	std::print("value: {}", t.x);
        }
        ```
  
  - This links to the bit on https://en.cppreference.com/w/cpp/language/copy_elision that discusses this fact with the following basic example:
  
    ```cpp
    T x = T(T(f())); // x is initialized by the result of f() directly; no move
    // If f were a direct call to T() that would also be elided 
    ```
  
    - Noting that in some cases **elision cannot apply**
  
    ```cpp
    struct C { /* ... */ };
    C f();
     
    struct D;
    D g();
     
    struct D : C
    {
        D() : C(f()) {}    // no elision when initializing a base-class subobject
        D(int) : D(g()) {} // no elision because the D object being initialized might
                           // be a base-class subobject of some other class
    };
    ```
  
    ```cpp
    struct Type {
    
    	Type() : x{ 0 } { std::print("default constructor"); }
    	explicit Type(int val) : x{ val } { std::print("int constructor\n");}
    	Type(const Type& obj) : x{ obj.x } { std::print("copy construct\n"); }
    	Type(Type&& obj) noexcept : x{obj.x} { std::print("move construct\n"); }
    
    	int x;
    };
    
    int main() {
    	Type t{ Type{2}}; // The construction of Type {2} and the move of its temporary are both elided resulting in a construction of Type directly from an integer.
    	std::print("value: {}", t.x);
    }
    ```
  
    - This extends to be anything is elided to directly replace the arguments initializing the r-value type, this is what C++17 idea of *deferred*

##### Misc reminders:

- A compiler knows all function local variables can be moved from.

- [Five advanced init techniques in C++](https://www.cppstories.com/2023/five-adv-init-techniques-cpp/)

- [Statics initialisation order](https://stackoverflow.com/questions/211237/static-variables-initialisation-order)

- `Constinit` - `Constinit` - Ensure a variable is initialised at compile time before linking ensuring global variables being used as **external linkage** specifiers specified by `inline` `extern` or `global` are not used before the **corresponding definition** is generated.

  - ```cpp
    #ifndef Fuck
    #define Fuck
    class FileSystem {
    public:
    	[[nodiscard]] static std::size_t numDisks();
    };
    #endif
    
    inline constinit FileSystem tfs;
    ```

  - ```cpp
    class Directory {
    public:
    	explicit Directory(int);
    };
    
    Directory::Directory(int) {
    	std::size_t disks = tfs.numDisks(); // guaranteed to b
    }
    ```

- [Copy list init vs direct list init](https://stackoverflow.com/questions/50422129/differences-between-direct-list-initialization-and-copy-list-initialization)

  - Copy list init , as via the rules of function overload resolution fails to compile when `explicit` is used for the constructor of said type.

  - C++17 Given some enumeration type you can not perform *copy-list-initalization* abut can *direct-list-initialisation*

    - ```CPP
      enum class Colour { RED, BLUE, YELLOW}
      
      Colour val = Colour::RED // errro
      ```

    - 


### Idioms

> [C++ Idioms](https://en.wikibooks.org/wiki/More_C%2B%2B_Idioms)

##### CRTP

```cpp
#include <cstdio>
 
#ifndef __cpp_explicit_this_parameter // Traditional syntax
 
template <class Derived>
struct Base { void name() { (static_cast<Derived*>(this))->impl(); } };
struct D1 : public Base<D1> { void impl() { std::puts("D1::impl()"); } };
struct D2 : public Base<D2> { void impl() { std::puts("D2::impl()"); } };
 
void test()
{
    // Base<D1> b1; b1.name(); //undefined behavior
    // Base<D2> b2; b2.name(); //undefined behavior
    D1 d1; d1.name();
    D2 d2; d2.name();
}
 
#else // C++23 alternative syntax; https://godbolt.org/z/s1o6qTMnP
 
struct Base { void name(this auto&& self) { self.impl(); } };
struct D1 : public Base { void impl() { std::puts("D1::impl()"); } };
struct D2 : public Base { void impl() { std::puts("D2::impl()"); } };
 
void test()
{
    D1 d1; d1.name();
    D2 d2; d2.name();
}
 
#endif
 
int main()
{
    test();
}
```

##### NVI

- [Non virtual interface](https://en.wikibooks.org/wiki/More_C%2B%2B_Idioms/Non-Virtual_Interface)
  - We basically have a *private method* in our *interface* which is a virtual method.
  - This ensures our base class can control pre and post conditions regarding a derived classes implementation of said virtual function.

##### SFINAE

> [SFINAE](https://en.wikibooks.org/wiki/More_C%2B%2B_Idioms/SFINAE) - wikibooks
>
> [SFINAE Reference](https://en.cppreference.com/w/cpp/language/sfinae) - cppreference

- Substitution failure is not an error.
- This rule applies during overload resolution of function templates: When substituting the explicitly specified or deduced type for the template parameter fails, the specialization is discarded from the overload set instead of causing a compile error. 

##### Pimpl

> [Pimpl Idiom](https://en.wikibooks.org/wiki/C%2B%2B_Programming/Idioms#Pointer_To_Implementation_(pImpl))

- Also called the **opaque pointer** idiom is a method of providing data and thus further implementation abstraction for Classes.

##### Erase-remove

- [Erase remove idiom](https://en.wikipedia.org/wiki/Erase%E2%80%93remove_idiom)

```cpp
// Use g++ -std=c++11 or clang++ -std=c++11 to compile.

#include <algorithm>  // remove and remove_if
#include <iostream>
#include <vector>

void Print(const std::vector<int>& vec) {
  for (auto val : vec) {
    std::cout << val << ' ';
  }
  std::cout << '\n';
}

int main() {
  std::vector<int> v = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
  Print(v);

  // Removes all elements with the value 5.
  v.erase(std::remove(v.begin(), v.end(), 5), v.end());
  Print(v);

  // Removes all odd numbers.
  v.erase(std::remove_if(v.begin(), v.end(), [](int val) { return val & 1; }),
          v.end());
  Print(v);
}

1 2 3 4 5
  2   4 >    13424  
/*				^
Output:
0 1 2 3 4 5 6 7 8 9
0 1 2 3 4 6 7 8 9
0 2 4 6 8
*/
```

- In C++20 We have the free function `std::free` which implements the erase remove idiom for us.

### Operators

- Operator precedence - https://www.learncpp.com/cpp-tutorial/operator-precedence-and-associativity/

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

- Floating point comparison at compile time

  - ```cpp
    // Prior to C++23 version
    #include <algorithm> // for std::max
    #include <iostream>
    
    // Our own constexpr implementation of std::abs (for use prior to C++23)
    // In C++23, use std::abs
    // constAbs() can be called like a normal function, but can handle different types of values (e.g. int, double, etc...)
    template <typename T>
    constexpr T constAbs(T x)
    {
        return (x < 0 ? -x : x);
    }
    
    // Return true if the difference between a and b is within epsilon percent of the larger of a and b
    constexpr bool approximatelyEqualRel(double a, double b, double relEpsilon)
    {
        return (constAbs(a - b) <= (std::max(constAbs(a), constAbs(b)) * relEpsilon));
    }
    
    // Return true if the difference between a and b is less than or equal to absEpsilon, or within relEpsilon percent of the larger of a and b
    constexpr bool approximatelyEqualAbsRel(double a, double b, double absEpsilon, double relEpsilon)
    {
        // Check if the numbers are really close -- needed when comparing numbers near zero.
        if (constAbs(a - b) <= absEpsilon)
            return true;
    
        // Otherwise fall back to Knuth's algorithm
        return approximatelyEqualRel(a, b, relEpsilon);
    }
    
    int main()
    {
        // a is really close to 1.0, but has rounding errors
        constexpr double a{ 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 };
    
        constexpr double relEps { 1e-8 };
        constexpr double absEps { 1e-12 };
    
        constexpr bool same { approximatelyEqualAbsRel(a, 1.0, absEps, relEps) };
        std::cout << same << '\n';
    
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

- Comma  `, `operator return type:

  - ```cpp
    auto func = [&](int x , int y) {return x + 1, y;} /* evalautes x+1 but returns y */
    ```

### Bit manipulation (review learncpp)

##### Misc reminders

- How bitwise OR represents flags

  - ```cpp
    BINARY DECIMAL COLOR
    ------ ------- -----
       001       1  Red
       010       2  Green
       011       3  Red+Green
       100       4  Blue
       101       5  Blue+Red
       110       6  Blue+Green
       111       7  Blue+Green+Red
    ```

  - So `Red` is an integer and so is `Blue` 

  - Bitwise OR `|` combines the two 

  - Therefore when we bitwise AND `&` check it evaluates true for both `Red` and `Blue` , alternatively as shown above `101` is the value stored from the bitwise OR which is represented in the program.

- C++20 Bit cast.

### Scope, Duration and Linkage

##### learncpp-misc

###### Linkage

<img src="./images/image-20231125002355696.png" alt="image-20231125002355696" style="zoom:67%;" />

- **No Linkage**: Identifier refers only to itself.
  - Local variables.
  - Type definitions (e.g., enums, classes) inside a block.
- **Internal Linkage**: Accessible within the file where declared.
  - Static global variables (both initialized and uninitialized).
  - Static functions.
  - Const global variables.
  - Functions in unnamed namespaces.
  - Type definitions (e.g., enums, classes) in unnamed namespaces.
- **External Linkage**: - Accessible in the file where declared and in other files.
  - Functions.
  - Non-const global variables (both initialized and uninitialized).
  - Extern const global variables.
  - Inline const global variables.

###### Duration

- Variables with **automatic duration** are created at the point of definition, and destroyed when the block they are part of is exited. This includes:
  - Local variables
  - Function parameters
- Variables with **static duration** are created when the program begins and destroyed when the program ends. This includes:
  - Global variables
  - Static local variables
- Variables with **dynamic duration** are created and destroyed by programmer request. This includes:
  - Dynamically allocated variables

###### scope

- An identifier’s *scope* determines where the identifier can be accessed within the source code.

  - Variables with **block (local) scope**  can only be accessed from the point of declaration until the end of the block in which they are declared (including nested blocks). This  includes:
    - Local variables
    - Function parameters
    - Program-defined type definitions (such as enums and classes) declared inside a block

  - Variables and functions with **global scope** can be accessed from the point of declaration until the end of the file. This includes:
    - Global variables
    - Functions
    - Program-defined type definitions (such as enums and classes) declared inside a namespace or in the global scope

##### Variable scope, duration, and linkage summary

![image-20231118150010756](images/image-20231118150010756.png)

##### Forward declaration summary

![image-20231118175150411](images/image-20231118175150411.png)

##### Misc reminders

- Global scope = file scope = global namespace scope

- Classes / Types exempt from ODR

  - Types declared in a header file can have multiple definitions amongst the files its included in

  - They should be the same definitions otherwise its undefined.

- **Extern** 

  - This comes in useful when you have global variables. You declare the *existence* of global variables in a header, so that each source file that includes the header knows about it, but you only need to “define” it once in one of your source files.
  - To clarify, using `extern int x;` tells the compiler that an object of type `int` called `x` exists *somewhere*. It's not the compilers job to know where it exists, it just needs to know the type and name so it knows how to use it. Once all of the source files have been compiled, the linker will resolve all of the references of `x` to the one definition that it finds in one of the compiled source files. For it to work, the definition of the `x` variable needs to have what's called “external linkage”, which basically means that it needs to be declared outside of a function (at what's usually called “the file scope”) and without the `static` keyword.
  - So we define the `extern` variable definition in one of the locations where the header is included and this compiles fine where each file including `header.h` can use the `global_x` without having to define it multiple times.

  ```cpp
  /* header.h */
  #ifndef HEADER_H
  #define HEADER_H
  
  // any source file that includes this will be able to use "global_x"
  extern int global_x;
  
  void print_global_x();
  
  #endif
  ```

  ```cpp
  /* main.cpp */
  
  #include "header.h"
  
  // since global_x still needs to be defined somewhere,
  // we define it (for example) in this source file
  int global_x;
  
  int main()
  {
      //set global_x here:
      global_x = 5;
  
      print_global_x();
  }
  ```

  ```cpp
  /* header.cpp (definition for our header)*/
  #include <iostream>
  #include "header.h"
  
  void print_global_x()
  {
      //print global_x here:
      std::cout << global_x << std::endl;
  }
  ```

- [When to use static variable in C++](https://stackoverflow.com/questions/25866893/when-to-use-static-variable-c)
  
  - **Global scope statics** -  for internal linkage, to avoid name conflicts between separate translation units
  - **Class scope statics** - internal class usage only, no conflicts between other classes.
  - Both Global and Class scope statics are *global*, given that for the class one you use `Class::`.
  - Another form of global variable , is just define a variable without `static` syntax. This becomes available to other files via the `extern` keyword.
  
- [The static keyword and its various uses in C++](https://stackoverflow.com/questions/15235526/the-static-keyword-and-its-various-uses-in-c)

  - Static free functions define internal linkage between TU. A note is that inline functions in a header already make these mostly redundant.

  - Each function having internal linkage allows the same name function to perform different operations in each TU.

- [Extern C Video](https://www.youtube.com/watch?v=m5Y3Ghv2PUE )

  - `extern "c"` $\to$ use c header libraries and link them properly without name mangling
    - Where `func()` in c++ actually becomes a symbol `_Z3func` which would cause a  

- [How do inline variables work](https://stackoverflow.com/questions/38043442/how-do-inline-variables-work)

  - Define a variable in a header as inline to be used amongst any translation unit, notably it solves the ODR (one definition rule) by its semantics.

- [static inline functions?](https://stackoverflow.com/questions/10876930/should-one-never-use-static-inline-function)

  - The `static` modifier at namespace scope was formerly deprecated in favour of unnamed namespaces

- **Translation unit** - A single file that is comprised of headers and .cpp file that will be compiled to a .obj

- Classes and other types are free from **ODR**
  - Types declared in a header file can have multiple definitions amongst the files its included in
  - They should be the same definitions otherwise its undefined.

- [MSCV types of scope](https://learn.microsoft.com/en-us/cpp/cpp/scope-visual-cpp?view=msvc-170)

### References, Pointers & Memory

##### github-course-misc

<img src="./images/image-20231128131906702.png" alt="image-20231128131906702" style="zoom:80%;" />

<img src="./images/image-20231128132124579.png" alt="image-20231128132124579" style="zoom: 80%;" />



```cpp
int data[] = {1,2}; //Data segment memory
int big_data[1000000] = {}; // BSS segment memory, zero initialized

int main(){
    int A = {1,2,3} // Stack memory
}
```

- A pointer can be explicitly converted to an integer type.

```cpp
void * x;
size_t y = (size_t) x; // ok its an explicit conversion.
```

- Adding `const` to a pointer is not the same as adding `const` to the type alias of a pointer

```cpp
using ptr_t = int*;
void f2(const ptr_t ptr){ // does not apply const to this pointer alias.
}
```

##### learn-cpp misc

- [**Member functions returning references to data members**](https://www.learncpp.com/cpp-tutorial/member-functions-returning-references-to-data-members/):

  - One notable thing is the idea that R-value references i.e the copy by value return of `createEmployee` returns a temporary (r value) that is destroyed after the expression it resides in has completed.

  - ```cpp
    #include <iostream>
    #include <string>
    #include <string_view>
    
    class Employee
    {
    	std::string m_name{};
    
    public:
    	void setName(std::string_view name) { m_name = name; }
    	const auto& getName() const { return m_name; } //  getter returns by const reference
    };
    
    // createEmployee() returns an Employee by value (which means the returned value is an rvalue)
    Employee createEmployee(std::string_view name)
    {
    	Employee e;
    	e.setName(name);
    	return e;
    }
    
    int main()
    {
    	// Case 1: okay: use returned reference to member of rvalue class object in same expression
    	std::cout << createEmployee("Frank").getName();
    
    	// Case 2: bad: save returned reference to member of rvalue class object for use later
    	const std::string& ref { createEmployee("Garbo").getName() }; // reference becomes dangling when return value of createEmployee() is destroyed
    	std::cout << ref; // undefined behavior
    
    	// Case 3: okay: copy referenced value to local variable for use later
    	std::string val { createEmployee("Hans").getName() }; // makes copy of referenced member
    	std::cout << val; // okay: val is independent of referenced member
    
    	return 0;
    }
    ```

- [R value references](https://www.learncpp.com/cpp-tutorial/rvalue-references/)
  - ![image-20231120012408049](images/image-20231120012408049.png)

- [L value References](https://www.learncpp.com/cpp-tutorial/lvalue-references/)

  - The following code snippet `ref1` actually refers to `var` itself when used to init `ref2`, it does not create a reference to a reference which is not valid C++ logic

  - ```cpp
    int var{};
    int& ref1{ var };  // an lvalue reference bound to var
    int& ref2{ ref1 }; // an lvalue reference bound to var
    ```

- **Always favour references to pointers whenever possible.** (https://www.learncpp.com/cpp-tutorial/null-pointers/)

- Other reference stuff:

  ```cpp
  
  // this is some bullshit showingn referecene materialisation
  float f = ...;
  const int& i = f;
  //is the same as
  float f = ...;
  int __i = f;
  const int& i = __i;
  ```

- Temporary / Conversion with reference binding:

  ```cpp
  int main() {
      const int& val0 = 2; // temporary + no conversion
      int& val1 = 3; // [error]
  
      int three = 3;
      const int& val2 = three; // no temporary + no conversion
      int& val3 = three; // no temporary + no conversion
  
      // both of these cases float has an conversion to int, three is materialized into a temporary
      // and under the hood binded to the converison oreator
      const float& val4 = three; // temporary + conversion
      const float val4b = threel // temporary + conversion
      float& val5 = three; // [error] 
      
      int int_val = 4;
      float&& val = int_val; // temporary + conversion
      const float&& val = int_cal // temporary +conversion
      
      return 0;
  }
  ```

##### cpp-core-guidelines

- [F.42 Return A T* to indicate only a position](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#f42-return-a-t-to-indicate-a-position-only)
  - Basically prefer a pointer if the position to return is not an ownership semantic and can be nullptr, otherwise prefer to use references always.

##### Misc reminders

- When we return by `const` for a temporary. This is redundant if its not a reference. 

  ```cpp
  const int& func1() {
  	int x{ 1 }; 
  	return x;
  }
  
  const int func2() {
  	int x{ 1 }; 
  	return x;
  }
  
  
  int main() {
  
  	int& val = func1(); // [error] cannot bind const int to int 
      int val = func2(); // [ok] drops the const okay
  }	
  ```

- Function pointer syntax:

  - https://cdecl.org

  - ```cpp
    *test(bool) // function pointer taking a bool as input
    void (*(*test(bool)))() // add parentheses around it then add a poonter and another pair of brackets
     //this is now a function pointer retunr a function pointer that takes no inputs and returns void
        
    // to extend remove void with another pointer and add another pointer layer placing bracket after return type of the other
        
     int(*(*(*test(bool)))())() 
    ```

  - this is a pointer to a function called test taking (bool) which returns a function pointer that points to another function pointer that returns int

- [When to pass by pointer vs reference](https://cplusplus.com/articles/z6vU7k9E/)

- [Differences between references and pointers](https://stackoverflow.com/questions/57483/what-are-the-differences-between-a-pointer-variable-and-a-reference-variable)

- [Shared Pointer vs Raw pointers](https://stackoverflow.com/questions/7657718/when-to-use-shared-ptr-and-when-to-use-raw-pointers)

- [R value references to arrays in C++](https://stackoverflow.com/questions/22562187/what-is-the-purpose-of-rvalue-reference-to-an-array-in-c11 )

- [Make Shared](https://en.cppreference.com/w/cpp/memory/shared_ptr/make_shared)
  
  - [Make Shared vs Normal shared](https://stackoverflow.com/questions/20895648/difference-in-make-shared-and-normal-shared-ptr-in-c)
  
- [References vs Pointers](https://stackoverflow.com/questions/7058339/when-to-use-references-vs-pointersv)

- [std:ref(T) vs T&](https://stackoverflow.com/questions/33240993/c-difference-between-stdreft-and-t)

- [Passing reference to a class constructor](https://stackoverflow.com/questions/4321305/best-form-for-constructors-pass-by-value-or-reference)

- `std::forward` - perfectly forwards the r value reference, l value reference. i.e it maintains the semantics of moving and referencing.

  - ```cpp
    
    template <typename T> func2(T&& arg){
        std::cout << "received arg as an r value reference\n";
    }
    
    template <typename T> func(T&& arg){
        func(std::forward<T>(arg));
    }
    
    int main(){
    
        int x {2};
        func(std::move(x));
    
    }
    ```

  - 

- `std::unique_ptr` vs `std::shared_ptr`  for pimpl idiom - https://stackoverflow.com/questions/5576922/should-i-use-shared-ptr-or-unique-ptr

- `std::unique_ptr` - generates a custom delete when there is no default destructor specified within the class its placed in.

- Placement New

  - [Placement New use cases](https://stackoverflow.com/questions/222557/what-uses-are-there-for-placement-new)

  - [Placement new explained as a memory pool calling explicit destructors](https://www.youtube.com/watch?v=2bsGFQgBMXs )

  - ```cpp
    class Arena {
          public:
                 void* allocate(size_t);
                  void deallocate(void*);
                  // ...
     };
    
    void* operator new(size_t sz, Arena& a){
        return a.allocate(sz);
    }
    
            Arena a1(some arguments);
            Arena a2(some arguments);
    ```

  - [Parameters to placement new](https://stackoverflow.com/questions/34800940/parameters-to-operator-new)

    - ```cpp
      void* operator new(std::size_t, /* extra parameters*/){
          /* do something then return a pointer to at least size bytes */
      }
      ```


  - Placement new in the context of `std::vector`
    - ![image-20231119184307716](images/image-20231119184307716.png)

- **Alignment**

  - [How to use alignof, alignas](https://stackoverflow.com/questions/17091382/memory-alignment-how-to-use-alignof-alignas)

    - Alignment is a *restriction* on which *memory positions* some values *first byte* can be stored. 

    - Alignment of `16` means the memory addresses that are **multiples** of `16` are the only valid addresses.

    - The `alignas` keyword forces alignment to the desired type of powers of `2` only

    - ```cpp
      #include <cstdlib>
      #include <iostream>
      
      int main() {
          alignas(16) int a[4];
          alignas(1024) int b[4]; /* rather than taking up 16 bytes, each one now pads up to 1024 bytes.*/
          printf("%p\n", a);
          printf("%p", b);
      }
      ```

    - ```cpp
      #include <iostream>
      
      /* the alignment is equal to the largest alignment data member
          every member must now follow this alignment rule and thus short would be padded 2 bytes to fit the
          4 byte alignment set by int
      */
      struct Foo2 /* alignment = 4 bytes, sizeof = 12 bytes for 3 members aligned to 4*/
      {
          short a{};/* 2 bytes*/
          /*2 padding bytes*/
          int b{}; /* 4 bytes*/
          short qq{}; /*2 bytes*/
         /*2 bytes*/
      };
      
      int main() {
          std::cout << sizeof(short) << std::endl;
          std::cout << sizeof(short) << std::endl;
          std::cout << sizeof(int) << std::endl;
          // total size of data members individually is 10 but as there must be padding to ensure proper alignment this changes.
          std::cout << sizeof(Foo2) << std::endl; // size is 12
      }
      ```

- **Returning by reference**

  - Do not return a reference to *non const local static* as it is defined in one place and accessed by multiple locations
  - assigning /init a normal variable with a returned reference performs a copy

  - object returned by reference must exist after the function returns.
  - Return by *address* is returning a **pointer** and thus shares similar caveats.
    - Useful for if we wish to **return** a `nullptr`. 
  
- **Reference binding** - Reference binding is the process by which references are bound to some function or object

- [learncpp - Reference binding rules](https://www.learncpp.com/cpp-tutorial/lvalue-references/)

  ```cpp
  int main(){
      
      // Rule 1 You cannot bind an r-value to a non const l-value reference. Reference binding rules state that l-value references must be bound to a modifiable lvalue, r-value references are constant and therefore this rule fails
      
      // Reason - r-values are non modifiable so the value of them can not be modified, its the semantics
      int val = 3;
      int& val = {std::move(val)} // illegal, you must bind a r-value reference to a const reference
      const int& val = std::move(val); // okay, we can bind any r-value (xvalue or pr value) to a const;
      
      // Rule 2 References cannot be reaseated (changed to refer to another object)
      int new_val = 4;
      int other_val = 5;
      int& val {new_val}; // okay val is now an alias for new_val
      val = other_val; // This is okay but it does not alter the reference `val` to refer to `other_val` , it changes `new_val` to be 5.
      
      //Rule 3 References must be initialised at their declaration
      int& ref; // [error] no value to initialise reference. 
  }
  ```

### Classes, OOP, ...

##### misc-cpp-core-guidelines

- [C.51: Use delegating constructors to represent common actions for all constructors of a class](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#c51-use-delegating-constructors-to-represent-common-actions-for-all-constructors-of-a-class)
- [C.35: A base class destructor should be either public and virtual, or protected and non-virtual](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-dtor-virtual)
- [C:127: A class with a virtual function should have a virtual or protected destructor](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#c127-a-class-with-a-virtual-function-should-have-a-virtual-or-protected-destructor)
- [C.164: Avoid implicit conversion operators](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#c164-avoid-implicit-conversion-operators)

##### Learn-cpp misc

- Inheritance access summary:

  ![image-20231122191408901](images/image-20231122191408901.png)

- [ref-qualifiers](https://www.learncpp.com/cpp-tutorial/ref-qualifiers/)

  - ```cpp
    #include <iostream>
    #include <string>
    #include <string_view>
    
    class Employee {
        std::string m_name;
    
    public:
        // Set the employee's name
        void setName(std::string_view name) { m_name = name; }
    
        // Getter with ref qualifier for lvalue objects - returns a const reference
        const auto& getName() const & { return m_name; } 
    
        // Getter with ref qualifier for rvalue objects - returns by value
        auto getName() const && { return m_name; } 
    };
    
    Employee createEmployee(std::string_view name) {
        Employee e;
        e.setName(name);
        return e;
    }
    
    int main() {
        Employee joe;
        joe.setName("Joe");
        std::cout << joe.getName() << '\n'; // Calls lvalue version
    
        std::cout << createEmployee("Frank").getName() << '\n'; // Calls rvalue version
        return 0;
    }
    ```

- [Members functions returning references to data members](https://www.learncpp.com/cpp-tutorial/member-functions-returning-references-to-data-members/)
- **Class invariant** - A **class invariant** is a condition that must be true  throughout the lifetime of an object in order for the object to remain  in a valid state. An object that has a violated class invariant is said  to be in an **invalid state**, and unexpected or undefined behaviour may result from further use of that object.

- [Delegating Constructors](https://www.learncpp.com/cpp-tutorial/delegating-constructors/)

  - ```cpp
    #include <iostream>
    #include <string>
    #include <string_view>
    
    class Employee
    {
    private:
        std::string m_name{};
        int m_id{ 0 };
    
    public:
        Employee(std::string_view name)
            : Employee{ name, 0 } // delegate initialization to Employee(std::string_view, int) constructor
        {
        }
    
        Employee(std::string_view name, int id)
            : m_name{ name }, m_id{ id } // actually initializes the members
        {
            std::cout << "Employee " << m_name << " created\n";
        }
    
    };
    
    int main()
    {
        Employee e1{ "James" };
        Employee e2{ "Dave", 42 };
    }
    ```

  - ```cpp
    #include <iostream>
    #include <string>
    #include <string_view>
    
    class Employee
    {
    private:
        static constexpr int default_id { 0 }; // define a named constant with our desired initialization value
    
        std::string m_name{};
        int m_id{ default_id }; // we can use it here
    
    public:
    
        Employee(std::string_view name, int id = default_id) // and we can use it here
            : m_name{ name }, m_id{ id }
        {
            std::cout << "Employee " << m_name << " created\n";
        }
    };
    
    int main()
    {
        Employee e1{ "James" };
        Employee e2{ "Dave", 42 };
    }
    ```

- Make any constructor taking a single argument `explicit` 
  - An exception is when the conversion is semantically equivalent and performant such as `std::string` to `std::string_view` 

##### Misc reminders

- `final` - The final keyword prevents inheriting from classes or overriding methods in derived classes.

- When you make a const constructed class. The this pointer is a pointer to a const object soooo , you cannot dereference that and alter its values as it affects the constness of your class.

- [Member access operators](https://en.cppreference.com/w/cpp/language/operator_member_access)

- [Memory layout of cpp object](https://www.vishalchovatiya.com/memory-layout-of-cpp-object/)

- Default destructors in a header interface:

  - ```cpp
    #pragma once
    #include <memory>
    class B;
    
    class A 
    {
    private:
      std::unique_ptr<B> my_b;
    public:
    // ~A() = default; 
    // defining the default destructor would fail as 
    // class B is still a partial class here
      ~A();
    };
    ```

  - ```cpp
    #include "A.hpp"
    
    // the previously forward declared class B is now included here
    #include "B.hpp"
    
    // we can use the default destructor here as B is
    // no longer a partial class
    A::~A() = default;  /* the forward declaration is overwritten at this point and thus this is a valid destructor implementation*/
    ```

- User declared, user defined, user provided:
  - ![image-20231121003940878](images/image-20231121003940878.png)

- [Implicit constructor vs default constructor](https://stackoverflow.com/questions/12340257/what-is-the-difference-between-implicit-constructors-and-default-constructors)

- Aggregate class has no (can also be templates)
  - user provided / explicit / inherited constructors
  - no private / protected non static data members
  - no virtual functions
  - no virtual,private protected base classes
- Implicit copy constructors deletion
  - Data members present that do not have a copy constructor defined on then , implicitly delete the copy constructors on the class they are a part of.

- **Virtual destructors** - Use to call destructor of a derived classed pointed to by the base class type.

- [**Virtual tables**](https://www.learncpp.com/cpp-tutorial/the-virtual-table/) - To implement virtual functions, C++ uses a special form of late binding known as the virtual table. The **virtual table** is a lookup table of functions used to resolve function calls in a  dynamic/late binding manner. The virtual table sometimes goes by other  names, such as *vtable*, *virtual function table*, *virtual method* table, or *dispatch table*.

  <img src="images/image-20231118150753871.png" alt="image-20231118150753871" style="zoom: 80%;" />

- [**Purpose of virtual functions**](https://stackoverflow.com/questions/2391679/why-do-we-need-virtual-functions-in-c)

- [Virtual functions vs pure virtual functions](https://stackoverflow.com/questions/2652198/difference-between-a-virtual-function-and-a-pure-virtual-function)

  > A virtual function makes its class a *polymorphic base class*. Derived classes can override virtual functions. Virtual functions  called through base class pointers/references will be resolved at  run-time. That is, the *dynamic type* of the object is used instead of its *static type*

  > A pure virtual function implicitly makes the class it is defined for *abstract* (unlike in Java where you have a keyword to explicitly declare the class abstract). Abstract classes **cannot be instantiated**. Derived classes **need to override/implement** all **inherited pure virtual functions**. If they **do not, they too will become abstract**. ends in `=0`
  
- [Protected vs Private](https://stackoverflow.com/questions/224966/what-is-the-difference-between-private-and-protected-members-of-c-classes)

- [What is the override keyword](https://stackoverflow.com/questions/18198314/what-is-the-override-keyword-in-c-used-for)

- [Default member values best practices](https://stackoverflow.com/questions/11594846/default-member-values-best-practice)

- [Using declarative vs using directive](https://stackoverflow.com/questions/16152750/using-directive-vs-using-declaration-swap-in-c)

- [Explicit for default constructors with two or more params](https://stackoverflow.com/questions/4467142/why-is-explicit-allowed-for-default-constructors-and-constructors-with-2-or-more)

  - ```cpp
    struct String {
        // this is a non-converting constructor
        explicit String(int initialLength, int capacity);
    };
    
    struct Address {
        // converting constructor
        Address(string name, string street, string city);
    };
    
    String s = { 10, 15 }; // error!
    String s1{10, 15}; // fine
    
    Address a = { "litb", "nerdsway", "frankfurt" }; // fine
    ```

- `Explicit` Keyword

  - Explicit Keyword in C++ is **used to mark constructors to not implicitly convert types in C++**. It is optional for constructors that take exactly one argument and works on constructors(with single argument) since those are the only constructors that can be used in type casting.

  - Use explicit constructors to ensure the type being converted is not implicitly changing into the class type.

- **Implicit conversions**

  ```cpp
  class balls {
      operator int(){
          return ...
      }
  }
  
  int x = balls() /* runs our implicitly user defined function*/
  ```

- **Mutable keyword** - permits modificationof the class member declared mutable even if the containing object is declared const (i.e., the class member is mutable)

  - Declare an object mutable meaning the object that is const can act on member functions using these values
    - Basically some variables are mutable and can change even if the object they are instantiated within is declare

  ```cpp
  class X
  {
      mutable const int* p; // OK
      mutable int* const q; // ill-formed
      mutable int&       r; // ill-formed
  };
  ```

- Passing this context to a lambda within  a class:
  - This gives the lambda access to the values of the class directly without having t

```cpp
[this,session](const boost::system::error_code& ec) {
			if (ec.value() != 0) {
				session->m_ec = ec;
				onRequestComplete(session);
				return;
			}
```

- [C++11 POD Standard layout definition](https://stackoverflow.com/questions/7160901/why-is-c11s-pod-standard-layout-definition-the-way-it-is)

  - A standard-layout class is a class that - https://en.cppreference.com/w/cpp/language/classes#Standard-layout_class:

  > 1. has no non-static data members of type non-standard-layout class (or array of such types) or reference,
  >
  > 2. has no virtual functions (10.3) and no virtual base classes (10.1),
  >
  > 3. has the **same access control** (Clause 11) for all non-static data members,
  >
  > 4. has no non-standard-layout base classes,
  >
  > 5. either has no non-static data members in the most derived class and **at most one base class with non-static data members**, or has no base classes with non-static data members, and
  >
  > 6. has no base classes of the **same type as the ﬁrst non-static data member**.

- [Enabled shared from this](https://en.cppreference.com/w/cpp/memory/enable_shared_from_this)

- `std::unique_ptr` generates its own custom deleter when the class its a member function of does not define a default destructor.

- [C++ non-standard-layout class layouts](https://quuxplusone.github.io/blog/2022/03/04/non-standard-layout-guarantees/)

  - You can cast a *standard layout class object*  address to a pointer of its first member:

    ```cpp
    struct A {int x;};
    
    A a;
    
    int *px = (int*) &a;
    
    A *pa = (A*)px;
    
    ```

- **[Effective C++ Item 9]** not call virtual functions within destructors, as the base class is created then derived therefore unexpected results could arise.

- We can use the concept of inline variables in a class to define a static variable in the class definition itself as opposed to on the outside.

  - ```cpp
    // Pre C++17
    #include<iostream>
    using namespace std;
    class MyClass {
       public:
          MyClass() {
             ++num;
          }
          ~MyClass() {
             --num;
          }
          static int num;
    };
    int MyClass::num = 10;
    int main() {
       cout<<"The static value is: " << MyClass::num;
    }
    ```

  - ```cpp
    // C++17 inline variables
    #include<iostream>
    using namespace std;
    class MyClass {
       public:
          MyClass() {
             ++num;
          }
          ~MyClass() {
             --num;
          }
          inline static int num = 10;
    };
    int main() {
       cout<<"The static value is: " << MyClass::num;
    }
    ```


- Special Members that are declared based on user declarations in C++ for a class:
  - <img src="images/image-20231119174850931.png" alt="image-20231119174850931" style="zoom: 80%;" />

- Prefer a default constructor that has no member init list, but rather initialises the data members directly:


  - ```cpp
    struct T{
        T() = default;
        int i {};
        bool toggle {false};
    };
    
    struct T {
        T() : i{}, toggle{false}{}
        int i;
        bool toggle;
    };
    ```

- **Virtual inheritance** - A solution to the *diamond problem*
  
  
  - [What is virtual inheritance and when to use in C++](https://www.sandordargo.com/blog/2020/12/23/virtual-inheritance)
  - [C++ Virtual base class](https://www.scaler.com/topics/virtual-base-class-in-cpp/)
  - [Virtual keyword for use in inheritance](https://stackoverflow.com/questions/14163924/virtual-keyword-in-inheritance )
  - <img src="images/image-20231120235039551.png" alt="image-20231120235039551" style="zoom:67%;" />
  - `B` and `C` inherit virtually therefore `D` does not contain duplicated members of `A`.
  
- **Ignoring virtualisation**

  - ```cpp
    #include <iostream>
    int main()
    {
        Derived derived;
        const Base& base { derived };
        // Calls Base::GetName() instead of the virtualized Derived::GetName()
        std::cout << base.Base::getName() << '\n';
    
        return 0;
    }
    ```

### Unions

##### Misc reminders

- [What is a union for in C and C++](https://stackoverflow.com/questions/2310483/purpose-of-unions-in-c-and-c)
- [Why use unions](https://stackoverflow.com/questions/4788965/when-would-anyone-use-a-union-is-it-a-remnant-from-the-c-only-days)

### Namespaces

##### Misc reminders:

- [Unnamed namespaces](https://stackoverflow.com/questions/357404/why-are-unnamed-namespaces-used-and-what-are-their-benefits) - internal linkage.

- [Namespace scope](https://stackoverflow.com/questions/16776293/understanding-namespace-scope-in-c)

- Using directive is referred to as bringing the namespace `N` from `using namespace N` into the scope for name resolution to occur.

- **Global namespace** - The top level namespace, accessed via just using the prepended :: operator - https://stackoverflow.com/questions/4269034/what-is-the-meaning-of-prepended-double-colon.

- Unqualified namespace calls - ADL - https://en.cppreference.com/w/cpp/language/adl

  - it will be found because unqualified calls will add the namespace of any class argument to the lookup - thus the name argument-dependent lookup.

  - ```cpp
    namespace mynamespace {
        class MyClass {};
        void swap(MyClass&, MyClass&);
    }
    mynamespace::MyClass a, b;
    swap(a, b);
    ```



##### Auto rules

- [Auto type deduction rules summary](https://blog.feabhas.com/2016/11/getting-head-around-autos-type-deduction-rules/)

  - ```cpp
    // Basic auto usage
    auto x = initialValue;  // x's type deduced from initialValue
    
    // Example: auto deducing int
    auto a = 17;            // a is int
    
    // Example: auto with brace initialization
    auto b {17};            // b is int
    
    // Auto and complex expressions
    auto it = v.begin();    // it's type deduced from v.begin()
    
    // Auto with const
    int const var = 178;
    auto c = var;           // c is int, const dropped
    
    // Auto with C-style arrays
    int my_array[10];
    auto d = my_array;      // d is int*, array decays to pointer
    
    // Auto with l-value references
    template<typename T>
    void fn(T& param) {}
    // param's type includes cv-qualifiers of the argument
    
    // Auto with r-value references
    template<typename T>
    void fn(T&& param) {}
    // param's type deduces to l-value or r-value reference based on argument
    
    // Reference collapsing rules
    T& & -> T&,
    T& && -> T&,
    T&& & -> T&,
    T&& && -> T&&
    
    // Guidelines
    // - Avoid auto for scalar types
    // - Use auto for complex types, especially from functions like std::make_shared
    // - Be cautious with auto and reference qualifiers
    // - Remember that cv-qualifiers on auto-deduced types apply to the
    
    ```

- `auto*`

  - ```cpp
    #include <string>
    
    const std::string* const getConstPtr(); // some function that returns a const pointer to a const value
    
    int main()
    {
        auto ptr1{ getConstPtr() };  // const std::string* (drops top level const then deduces a const std::string*)
        auto* ptr2{ getConstPtr() }; // const std::string*
        
        /* reapply the const after its dropped as being the top level*/
        auto const ptr3{ getConstPtr() };  // const std::string* const
        const auto ptr4{ getConstPtr() };  // const std::string* const
    
        
        auto* const ptr5{ getConstPtr() }; // const std::string* const
        
        /* actually useless as this is deduced to const*/
        const auto* ptr6{ getConstPtr() }; // const std::string*
    
        /* const already deduced */
        const auto const ptr7{ getConstPtr() };  // error: const qualifer can not be applied twice
        
        /* top level dropped but added back , low level const added as documentation*/
        const auto* const ptr8{ getConstPtr() }; // const std::string* const
    
        return 0;
    }
    ```

##### learncpp-misc

- *Integer* refers to the `int` datatype, whereas *Integral* (https://en.cppreference.com/w/cpp/types/is_integral) is more broad and can include `bool` 

- Avoid literals and static casts when dealing with type conversions:

  ```cpp
   // We can avoid literals with suffixes
   unsigned int u{ 5 }; // okay (we don't need to use `5u`)
   float f{ 1.5 };      // okay (we don't need to use `1.5f`)
  
   // We can avoid static_casts
   constexpr int n{ 5 };
   double d{ n };       // okay (we don't need a static_cast here)
   short s{ 5 };        // okay (there is no suffix for short, we don't need a static_cast here)
  
   return 0;
  ```

- Signed integer:

  - Overflow is **undefined behaviour** in C++ for signed integers

    - ```cpp
      #include <iostream>
      
      int main()
      {
          // assume 4 byte integers
          int x { 2'147'483'647 }; // the maximum value of a 4-byte signed integer
          std::cout << x << '\n';
      
          ++x; // integer overflow, undefined behavior
          std::cout << x << '\n';
      
          return 0;
      }
      ```

  - ![image-20231122193549116](images/image-20231122193549116.png)

  - ![image-20231122193518341](images/image-20231122193518341.png)

  - Be careful when using integer division, as you will lose any fractional  parts of the quotient. However, if it’s what you want, integer division  is safe to use, as the results are predictable.

- [object-sizes-and-the-sizeof-operator](https://www.learncpp.com/cpp-tutorial/object-sizes-and-the-sizeof-operator/)

  - ![image-20231120012922209](images/image-20231120012922209.png)

- Fixed width integers (**prefer**):

  - ![image-20231120012953506](images/image-20231120012953506.png)

  - Fast and least integers:

    - ```cpp
      #include <cstdint> // For fast and least integer types
      #include <iostream>
      
      int main() {
          // Demonstrating sizes of least integer types
          std::cout << "least 8:  " << sizeof(std::int_least8_t) * 8 << " bits\n";
          std::cout << "least 16: " << sizeof(std::int_least16_t) * 8 << " bits\n";
          std::cout << "least 32: " << sizeof(std::int_least32_t) * 8 << " bits\n";
          std::cout << '\n';
      
          // Demonstrating sizes of fast integer types
          std::cout << "fast 8:  " << sizeof(std::int_fast8_t) * 8 << " bits\n";
          std::cout << "fast 16: " << sizeof(std::int_fast16_t) * 8 << " bits\n";
          std::cout << "fast 32: " << sizeof(std::int_fast32_t) * 8 << " bits\n";
      
          // Example of behavior variation based on size of fast integer type
          std::uint_fast16_t sometype { 0 };
          --sometype; // Overflow to invoke wraparound behavior
          std::cout << "Behavior example: " << sometype << '\n';
      
          return 0;
      }
      
      // Comments:
      // - `std::int_least#_t` and `std::uint_least#_t` represent the smallest types with at least # bits.
      // - `std::int_fast#_t` and `std::uint_fast#_t` represent the fastest types with at least # bits.
      // - The actual size of fast types may vary, potentially causing different behaviors on different architectures.
      // - Demonstrated by showing sizeof() output and a behavior example with wraparound in fast integers.
      ```

- `std::uint8_t` and `std::int8_t` may behave like `char` on some systems which can be a cause of errors.

- `std::size_t` is an unsigned integral type and typically represents the size or length of objects.
  - Some compilers limit the largest creatable object to half the maximum value `std::size_t` 

- A **cv unqualified** type has no const or volatile applied to it i.e its raw type.

- **Literals**:

  - ![image-20231120213205828](images/image-20231120213205828.png)
  - ![image-20231120213125458](images/image-20231120213125458.png)

- C++ Numeral systems:

  - ```cpp
    // Numeral Systems in C++ (Decimal, Binary, Hexadecimal, Octal)
    
    // Decimal (Base 10)
    int x { 12 }; // Decimal number
    
    // Binary (Base 2)
    // Counting in binary: 0, 1, 10, 11, 100, 101, 110, 111, ...
    
    // Octal (Base 8)
    // Counting in octal: 0, 1, 2, 3, 4, 5, 6, 7, 10, 11, 12, ...
    // Octal literal example
    int octal_num { 012 }; // Equals 10 in decimal
    
    // Hexadecimal (Base 16)
    // Counting in hexadecimal: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F, 10, ...
    // Hexadecimal literal example
    int hex_num { 0xF }; // Equals 15 in decimal
    
    // Binary literals in C++14
    int bin {};
    bin = 0b1;        // Binary 1
    bin = 0b11;       // Binary 3
    bin = 0b1010;     // Binary 10
    
    // Digit Separators (C++14)
    long value { 2'132'673'462 }; // Easier to read
    
    // Outputting values in different formats
    std::cout << std::hex << x << '\n'; // Output in hexadecimal
    std::cout << std::oct << x << '\n'; // Output in octal
    std::cout << std::dec << x << '\n'; // Output in decimal
    
    // Outputting binary values using std::bitset
    #include <bitset>
    std::bitset<8> bin1{ 0b1100'0101 };
    std::cout << bin1 << '\n';
    
    // Using std::format in C++20 and C++23
    #include <format>
    std::cout << std::format("{:b}\n", 0b1010); // Output in binary
    
    // Compiler requirements for std::format and std::print
    // MSVC: Visual Studio 2019 or later
    // g++ (GCC): GCC 13 or later
    ```

- Promotions / Conversions 

  - [Narrowing Conversions, List init and constexpr init](https://www.learncpp.com/cpp-tutorial/narrowing-conversions-list-initialization-and-constexpr-initializers/)

- Narrowing conversions -> **compiler emits a warning normally**:

  - Avoid by using `{}`, enforcing the **precision is important.**
  - From a floating point type to an integral type.
  - From a floating point type to a narrower or lesser ranked floating point type,  unless the value being converted is constexpr and is in range of the  destination type (even if the destination type doesn’t have the  precision to store all the significant digits of the number).
  - From an integral to a floating point type, unless the value being converted  is constexpr and whose value can be stored exactly in the destination  type.
  - From an integral type to another integral type that cannot represent all values of the original type, unless the value being  converted is constexpr and whose value can be stored exactly in the  destination type. This covers both wider to narrower integral  conversions, as well as integral sign conversions (signed to unsigned,  or vice-versa).
  - **Because they can be unsafe and are a source of errors, avoid narrowing conversions whenever possible.**
  - **If you need to perform a narrowing conversion, use `static_cast` to convert it into an explicit conversion.**

- List initialization with constexpr initializers:

  - These constexpr exception clauses are incredibly useful when list  initializing non-int/non-double objects, as we can use an int or double  literal (or a constexpr object) initialization value.

    ```cpp
    int main()
    {
        // We can avoid literals with suffixes
        unsigned int u { 5 }; // okay (we don't need to use `5u`)
        float f { 1.5 };      // okay (we don't need to use `1.5f`)
    
        // We can avoid static_casts
        constexpr int n{ 5 };
        double d { n };       // okay (we don't need a static_cast here)
        short s { 5 };        // okay (there is no suffix for short, we don't need a static_cast here)
    
        return 0;
    }
    ```

- **Integral promotion** rules and floating point promotion:

  - floating point -> float goes to double.
  - Using the **integral promotion** rules, the following conversions can be made:
    - signed char or signed short can be converted to int.
    - unsigned char, char8_t, and unsigned short can be converted to int if int can  hold the entire range of the type, or unsigned int otherwise.
    - If char is signed by default, it follows the signed char conversion rules  above. If it is unsigned by default, it follows the unsigned char  conversion rules above.
    - bool can be converted to int, with false becoming 0 and true becoming 1.

- **Arithmetic conversion** rules:

  - The usual arithmetic conversion rules are pretty simple. The compiler has a prioritized list of types that looks something like this:

    - long double (highest)

    - double

    - float

    - unsigned long long

    - long long

    - unsigned long

    - long

    - unsigned int

    - int (lowest)

      - **There are only two rules:**

        - If the type of at least one of the operands is on the priority list, the  operand with lower priority is converted to the type of the operand with higher priority.

        - Otherwise (the type of neither operand is on the list), both operands are numerically promoted (see [10.2 -- Floating-point and integral promotion](https://www.learncpp.com/cpp-tutorial/floating-point-and-integral-promotion/)).


- Floating point - https://float.exposed/0x3dcccccd:

  - ![image-20231122193000672](images/image-20231122193000672.png)

  - ![image-20231122192929810](images/image-20231122192929810.png)

  - Always make sure the type of your literals match the type of the  variables they’re being assigned to or used to initialize. Otherwise an  unnecessary conversion will result, possibly with a loss of precision.
  - Floating point numbers are useful for **storing very large or very small numbers**, including those with fractional components.
  - Floating point numbers often have small rounding errors, even when the number  has fewer significant digits than the precision. Many times these go  unnoticed because they are so small, and because the numbers are  truncated for output. However, comparisons of floating point numbers may not give the expected results. Performing mathematical operations on  these values will cause the rounding errors to grow larger.
  - Favor double over float unless space is at a premium, as the lack of precision in a float will often lead to inaccuracies.

- Unsigned integers

  - More details - https://blog.libtorrent.org/2016/05/unsigned-integers/

  - C++ standard says unsigned integers are well defined in overflowing from arithmetic or storage operations.

  - Unsigned integers exhibit **wrap around behaviour** 

    ```cpp
    #include <iostream>
    
    int main()
    {
        unsigned short x{ 0 }; // smallest 2-byte unsigned value possible
        std::cout << "x was: " << x << '\n';
    
        x = -1; // -1 is out of our range, so we get modulo wrap-around
        std::cout << "x is now: " << x << '\n';
    
        x = -2; // -2 is out of our range, so we get modulo wrap-around
        std::cout << "x is now: " << x << '\n';
    
        return 0;
    }
    
    x was: 0
    x is now: 65535
    x is now: 65534
    ```

  - Favor signed numbers over unsigned numbers for holding quantities (even  quantities that should be non-negative) and mathematical operations.  

  - Avoid mixing signed and unsigned numbers.

    ![image-20231122193638575](images/image-20231122193638575.png)

##### Misc reminders

- When you bind a const T& to a T, there is no conversion going on.

- C++ types - https://en.cppreference.com/w/cpp/language/types

- Prefer fixed width integers for most cases
  - `std::uint8_t` for chars
  - `std::size_t` for sizes i.e unsigned integer type dependent on the architecture
  - Prefer larger unsigned integers for varying contexts
- Do not mix signed and unsigned integers.
- Prefer `int` for simple numbers

- `constexpr` 

  - Implies const

  - helps for performance and memory usage

  - could impact compile time.

- [LEARNCPP](https://www.learncpp.com/cpp-tutorial/constexpr-and-consteval-functions/) **Constexpr / Consteval**

  - Unless you have a specific reason not to, a function that can be made `constexpr` generally should be made `constexpr`.

  - Use `consteval` if you have a function that must run at compile-time for some reason (e.g. performance).

    - ```cpp
      #include <iostream>
      
      // Uses abbreviated function template (C++20) and `auto` return type to make this function work with any type of value
      // See 'related content' box below for more info (you don't need to know how these work to use this function)
      consteval auto compileTime(auto value)
      {
          return value;
      }
      
      constexpr int greater(int x, int y) // function is constexpr
      {
          return (x > y ? x : y);
      }
      
      int main()
      {
          std::cout << greater(5, 6) << '\n';              // may or may not execute at compile-time
          std::cout << compileTime(greater(5, 6)) << '\n'; // will execute at compile-time
      
          int x { 5 };
          std::cout << greater(x, 6) << '\n';              // we can still call the constexpr version at runtime if we wish
      
          return 0;
      }
      ```

  - The compiler must be able to see the full definition of a constexpr or consteval function, not just a forward declaration.
  - Constexpr/consteval functions used in a single source file (.cpp) can be defined in the source file above where they are used.
  - Constexpr/consteval functions used in multiple source files should be defined in a header  file so they can be included into each source file.


- [What are forward declarations in C++](https://stackoverflow.com/questions/4757565/what-are-forward-declarations-in-c)
- [What is the purpose of forward declarations in C++](https://stackoverflow.com/questions/3110096/what-is-the-purpose-of-forward-declaration)
- [When to use forward declarations in C++ ](https://stackoverflow.com/questions/553682/when-can-i-use-a-forward-declaration)

- [What are cv qualifiers in C++](https://stackoverflow.com/questions/27527642/what-does-cv-qualified-mean)
- [Volatile keyword](https://stackoverflow.com/questions/4437527/why-do-we-use-volatile-keyword)

```cpp
int test {100};
volatile int test {100}; /* does not optimise.*/

while (test == 100){ /* optimize to while(true) may be undesirable*/
 ...
}
```

- `Using` vs `Typedef` in the context of function pointers.

```cpp
typedef  void(*fp)(int); /* fp now alias to this function pointer*/
using fp = void(*)(int); /* fp now alias*/

// You can have calling conventions added too

using fp = void(__stdcall *)(int); // use msvc __stdcall calling convention
    
    
void func(int x){}
fp {&func};
```

- [RTTI](https://en.cppreference.com/w/cpp/types) + [Microsoft MSVC RTTI](https://learn.microsoft.com/en-us/cpp/cpp/run-time-type-information?view=msvc-170)

- `constexpr` functions are *implicitly inline*

  - Because constexpr functions may be evaluated at compile-time, the  compiler must be able to see the full definition of the constexpr  function at all points where the function is called.
  - This means that a constexpr function called in multiple files needs to have its  definition included into each such file -- which would normally be a  violation of the one-definition rule. To avoid such problems, constexpr  functions are implicitly inline, which makes them exempt from the  **one-definition rule**.

- When is a function ran at compile time

  - According to the C++ standard, a `constexpr` function that is eligible for compile-time evaluation *must* be evaluated at compile-time if the return value is used where a constant expression is required. Otherwise, the compiler is free to  evaluate the function at either compile-time or runtime.

  - ```cpp
    #include <iostream>
    
    constexpr int greater(int x, int y)
    {
        return (x > y ? x : y);
    }
    
    int main()
    {
        constexpr int g { greater(5, 6) };            // case 1: evaluated at compile-time
        std::cout << g << "is greater!";
    
        int x{ 5 }; // not constexpr
        std::cout << greater(x, 6) << " is greater!"; // case 2: evaluated at runtime
    
        std::cout << greater(5, 6) << " is greater!"; // case 3: may be evaluated at either runtime or compile-time
    
        return 0;
    }
    ```

- `consteval` 

  - Notes that the function must be evaluated at compile time, otherwise it results in an error.

  - ```cpp
    #include <iostream>
    
    consteval int greater(int x, int y) // function is now consteval
    {
        return (x > y ? x : y);
    }
    
    int main()
    {
        constexpr int g { greater(5, 6) };            // ok: will evaluate at compile-time
        std::cout << greater(5, 6) << " is greater!"; // ok: will evaluate at compile-time
    
        int x{ 5 }; // not constexpr
        std::cout << greater(x, 6) << " is greater!"; // error: consteval functions must evaluate at compile-time
    
        return 0;
    }
    ```

  - ```cpp
    #include <iostream>
    
    // Uses abbreviated function template (C++20) and `auto` return type to make this function work with any type of value
    // See 'related content' box below for more info (you don't need to know how these work to use this function)
    consteval auto compileTime(auto value)
    {
        return value;
    }
    
    constexpr int greater(int x, int y) // function is constexpr
    {
        return (x > y ? x : y);
    }
    
    int main()
    {
        std::cout << greater(5, 6);              // may or may not execute at compile-time
        std::cout << compileTime(greater(5, 6)); // will execute at compile-time
    
        int x { 5 };
        std::cout << greater(x, 6);              // we can still call the constexpr version at runtime if we wish
    
        return 0;
    ```

- **Auto type decay**

  - ```cpp
    constexpr int i = 42;
    const int& ir = i;
    auto a = ir; /* takes away the const and & of ir making it just a normal int*/
    ```

### Expressions

##### Value Categories

- https://stackoverflow.com/questions/3601602/what-are-rvalues-lvalues-xvalues-glvalues-and-prvalues

- [Excellent timestamp from templates](https://youtu.be/2Y9XbltAfXs?si=NuwedA6F4KgACLxn&t=1258)

  - `xvalue` - An expiring value, i.e a value that has been moved from an and is an invalid state. This is *cast* to an *rvalue reference*	
    - Example `std::move(x)` where `x` is an `lvalue`. 
  
  - `rvalue` - anything that is a `prvalue` or `xvalue`.
  
  - `prvalue` - Literals that are not user-defined literals or string literals
  
    - Applications of built-in arithmetic operators
    - A call to a function with non-reference return type
  - `lvalue` - `glvalues` that are **not xvalues**.
  
  <img src="images/image-20231120232056349.png" alt="image-20231120232056349" style="zoom:67%;" />

- [Order of Evaluation](https://en.cppreference.com/w/cpp/language/eval_order)

##### Misc reminders

- **Static cast** - This is the simplest type of cast which can be used. It is a **compile time cast**. It does things like implicit conversions between types (such as int to float, or pointer to void*), and it can also call explicit conversion functions (or implicit ones).
  
  - `static_cast<int>(x) vs int(x)`-  (https://stackoverflow.com/questions/103512/why-use-static-castintx-instead-of-intx)
  
- **Types of casting**

  <img src="images/image-20231119192132084.png" alt="image-20231119192132084" style="zoom:80%;" />

### Functions

##### Learncpp-misc

- **How overloaded functions are differentiated:**

  - ![image-20231120222954824](images/image-20231120222954824.png)

  - ![image-20231120223009926](images/image-20231120223009926.png)

- [Function overload resolution](https://www.learncpp.com/cpp-tutorial/function-overload-resolution-and-ambiguous-matches/)

  - ```cpp
    #include <iostream>
    #include <string>
    
    // Function overload resolution is crucial when multiple functions have the same name but different parameters.
    // The compiler uses a series of steps to determine the best match for a function call.
    
    // Example of simple overload resolution
    void print(int x) { std::cout << x << '\n'; }
    void print(double d) { std::cout << d << '\n'; }
    
    // The compiler follows these steps for resolving overloaded functions:
    // 1. Exact match: Checks if function call matches any overloaded function's parameters exactly.
    // 2. Trivial conversions: Converts non-const to const, non-reference to reference, etc.
    // 3. Numeric promotion: Promotes narrow types (char, short) to wider types (int, double).
    // 4. Numeric conversions: Converts between different numeric types (int to double, etc.).
    // 5. User-defined conversions: Invokes conversions defined in classes.
    // 6. Ellipsis matching: Matches functions with ellipsis (...) if no other match is found.
    
    // Ambiguous matches occur when multiple overloads match equally well in the same step.
    // This results in a compile error and needs to be resolved by the programmer.
    
    // Resolving Ambiguous Matches:
    // - Define an overload that exactly matches the function call.
    // - Explicitly cast arguments to match the desired overload.
    // - Use literal suffixes to specify the type of literal arguments.
    
    // Example of resolving ambiguous matches
    void resolveAmbiguousMatches() {
        print(5L); // Ambiguous: long can convert to int or double.
        print(static_cast<unsigned int>(0)); // Explicit cast resolves ambiguity.
        print(0u); // Literal suffix 'u' indicates unsigned int.
    }
    
    // Handling Functions with Multiple Arguments:
    // - The best match is chosen based on a combination of conversions applied to each argument.
    // - A function is selected if it provides a better match for at least one parameter and no worse for others.
    // - If no such function exists, the call is considered ambiguous.
    
    void print(char, int) { std::cout << 'a' << '\n'; }
    void print(char, double) { std::cout << 'b' << '\n'; }
    void print(char, float) { std::cout << 'c' << '\n'; }
    
    void multiArgumentExample() {
        print('x', 'a'); // Resolves to print(char, int) due to promotion of 'a' to int.
    }
    
    int main() {
        print(5);    // Calls print(int)
        print(6.7);  // Calls print(double)
    
        resolveAmbiguousMatches();
        multiArgumentExample();
    
        return 0;
    }
    ```

##### Misc reminders

- References are **not objects** so when a class returns a reference its not returning an object.

- When returning, looking at cppref we observe there 2 cases
  - Evaluates the expression, terminates the current function and returns the result of the expression to the caller, after [implicit conversion](https://en.cppreference.com/w/cpp/language/implicit_cast) to the function return type. The expression is optional in functions whose return type is (possibly cv-qualified) void, and disallowed in constructors and in destructors.
  - A braced init list that uses **copy list initialisation** to the return type.

- Mental model of a function

  ```cpp
  int func (){
      return 1;
  }
  
  int& func2(){
      int x {1};
      return x;
  }
  
  int* func3(){
      int x {1};
      return &x;
  }
  
  // extremely rare and pretty much never used, this is used in std::forward and std::forward as a `cast` 
  // to interpret the passed in lvalue as an expiring xvalue (std::move) but std::forward maintains the value category via reference collapsing based on the deduce return type (see c++11 modern cheatsheet for more information)
  int&& func4(){
      
  }
  
  int main(){
      func(); // func returns temporary, this is an r-value 1
      
      func2(); // this returns an alias for the internal variable x (note in our example x would be destroyed after this line)
      
      func3(); // this returns a temporary of int pointer type
     
  }
  ```

  - Key idea with functions is the `return` statement defines some entity, of which must be able to represented (underlying bits) in the return type without causing any issues
    - dropping const / volatile qualifiers
    - Losing precision 
    - Just any general bad conversions
  - When the function returns there are a few cases:
    - Pointer > pointer temporary is returned
    - Reference > ref alias is returned (maybe copied impl defined)
    - Value > temporary value is returned (Copy elision [**see in initialisation section for more details**] can be detected in cases to avoid the temporary value being materialized, and used only at the point of where that temporary was destined to be stored)
  - When you use the result of a function you are binding it to some other place such as constructor, assignment or function call, but the result of a function by itself is its own type, typically a temporary as explained above, but references being the nuance case.

- [When to use inline functions](https://stackoverflow.com/questions/1932311/when-to-use-inline-function-and-when-not-to-use-it)
  
  - When the compiler inline-expands a function call, the function’s code gets inserted into the caller’s code stream (conceptually similar to what happens with a [`#define` macro](https://isocpp.org/wiki/faq/inline-functions#inline-vs-macros)). This can, [depending on a zillion other things](https://isocpp.org/wiki/faq/inline-functions#inline-and-perf), improve performance, because the optimizer can [procedurally integrate](https://isocpp.org/wiki/faq/inline-functions#procedural-integration) the called code — optimize the called code into the caller.
  
- Overload Resolution - https://en.cppreference.com/w/cpp/language/overload_resolution
  - [Argument dependent lookup (ADL)](https://en.cppreference.com/w/cpp/language/adl)
  - [What exactly is ADL](https://stackoverflow.com/questions/8111677/what-is-argument-dependent-lookup-aka-adl-or-koenig-lookup)
  
- [`inline` functions vs preprocessor macros](https://stackoverflow.com/questions/1137575/inline-functions-vs-preprocessor-macros)

- **Mutable lambdas**

  - ```cpp
    include <iostream>
    #include <functional>
    
    void myInvoke(const std::function<void()>& fn)
    {
        fn();
    }
    
    int main()
    {
        int i{ 0 };
    
        // Increments and prints its local copy of @i.
        auto count{ [i]() mutable {
          std::cout << ++i << '\n';
        } };
    
        myInvoke(count); // 1
        myInvoke(count); // 2
        myInvoke(count); // 3
    
        return 0;
    }
    ```

- Comma operator return type:

  - ```cpp
    auto func = [&](int x , int y) {return x + 1, y;} /* evalautes x+1 but returns y */
    ```

- `std::bind` - [What is std::bind in C++](https://thispointer.com/stdbind-tutorial-and-usage-details/#:~:text=std%3A%3Abind%20is%20a,passed%20function%20bound%20or%20rearranged)

```cpp
void add(int first, int second, const char* string ) {
	
	std::cout << "first: " << first << "| second : " << second << std::endl;
	std::cout << "word :" << string << std::endl;

}
auto func = std::bind(&add, std::placeholders::_1, std::placeholders::_2, "test");

/* can call func, using our default "test" string on each value*/
```

### Arrays (std::array & C style arrays)

##### Misc reminders

- [Raw C arrays vs std::array performance](https://stackoverflow.com/questions/30263303/stdarray-vs-array-performance)

- C style array used in templates:

  - ```cpp
    template<typename T, size_t size>
    size_t GetSize(T(&arr)[size])
    {
        return size;
    }
    
    double arr[] = { 5.0, 6.0, 7.0, 8.0 };
    std::cout << GetSize<double>(arr) << std::endl;
    ```

  - Templated version using normal C++

  - ```cpp
    #include <iostream>
    #include <array>
    
    template<typename T>
    size_t GetSize(T& arr)
    }
        return std::size(arr);
    }
    
    int main() {
        double arr[] = { 5.0, 6.0, 7.0, 8.0 };
        /* arr > decays to pointer > reference taken in by template */
        std::cout << GetSize(arr) << std::endl;
     
    }
    ```

- [Passing  C++ Arrays to function by reference](https://www.nextptr.com/question/a6212599/passing-cplusplus-arrays-to-function-by-reference)

- Note on arrays memory address being reference to a `char [5]`

  - ```cpp
    //'array' is a reference to char [5]
    char (&array) [5]; 
    ```

  - ```cpp
    //Alias of a char[5]
    using FiveCharCode = char[5];
    
    //'code' is a char(&)[5]
    
    /* reference to a char[5] identfied as code*/
    void Bar(const FiveCharCode& code) {
     for(char c : code) { //range-based-for loop works
      std::cout << c << "\n";
     }
    }
    
    
    int main() {
     char code[5] = {'A','B','C','D','E'};
      //Call Bar
     Bar(code); //No explicit length passed
     return 0;
    }
    ```

  - Using generic return type and collection type. This notably uses the idea that they all can use ranged based for loops to operate.

  ```cpp
  /* specify collection type and then pass the collection*/
  template<typename _Ret, typename _Coll> typ_Ret Sum(const _Coll& c) {
  
   _Ret sum = 0;
   for(auto& v : c)
      sum += v;
   return sum;
  }
  
  int main() {
   //With regular array. Passed as reference
   int arr[] = {1,2,3,4,5};
   std::cout << Sum<int64_t>(arr) << "\n"; //15
  
   //With vector
   std::vector<int> vec = {1,2,3,4,5};
   std::cout << Sum<int64_t>(vec) << "\n"; //15
   return 0;
  }
  ```

- When you use `sizeof(arr)` the `arr` does not get converted to a pointer therefore this operation correctly obtains the size of bytes the array occupies in total.

### Standard Library

##### Learncpp-misc

- Randomization
  - ![image-20231120220826234](images/image-20231120220826234.png)

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

### Templates 

##### cppcon

- [Back 2 Basics Templates watch both parts](https://www.youtube.com/watch?v=XN319NYEOcE)

**Concepts**

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

### Concurrency

##### Misc reminders

- [**std::atomic**](https://stackoverflow.com/questions/31978324/what-exactly-is-stdatomic)





