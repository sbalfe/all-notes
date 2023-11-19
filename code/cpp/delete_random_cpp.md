# deleted random

###### Template External

- Must scope using `::` to access templates from a different file, accessing it from the global namespace.
- some standard functions may share the same name and thus this is vital.

###### init static data members in classes using inline keyword for variables

https://www.tutorialspoint.com/how-do-inline-variables-work-in-cplusplus-cplusplus17

```c++
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

###### placement new c++

https://stackoverflow.com/questions/222557/what-uses-are-there-for-placement-new

https://www.youtube.com/watch?v=2bsGFQgBMXs - placement new explained as a memory pool calling explicit destructors.

```c++
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

###### parameters to placement new

https://stackoverflow.com/questions/34800940/parameters-to-operator-new

```c++
void* operator new(std::size_t, /* extra parameters*/){
    /* do something then return a pointer to at least size bytes */
}
```

###### std::vector placement new 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220317005925061.png)

- for some `std::vector<T>` 

###### Passing types to templates

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220317025346310.png)

###### Linkage 

- `external` $\to$ share definitions between multiple translation units
  - `extern` keyword. ensure the function is known to be declaration
- `translation unit` $\to$ single compiled object files comprised of `.cpp` and`.h` or any `#includes` 
- `internal` $\to$ local for a single translation unit
- `extern "c"` $\to$ use c header libraries and link them properly without name mangling
  - Where `func()` in c++ actually becomes a symbol `_Z3func` which would cause a  
- `inline` $\to$ compiler expects a definition in this case.
  - https://bit.ly/3qnGlNk - CPP header file rules.
- `inline variables` - https://stackoverflow.com/questions/38043442/how-do-inline-variables-work
  - inline variables define a variable in a header as inline to be used amongst any translation unit

https://www.youtube.com/watch?v=m5Y3Ghv2PUE - extern c 

```c++
/* header.h */

extern int y; /* any file including "header.h" , only one can define the value of y used in other ones*/
inline int x {3}; /* inline header definition ensures anyone including always gets this same value */
extern void func(); /* same as variable just check for a definition.*/
```



###### comma operator return type

```c++
auto func = [&](int x , int y) {return x + 1, y;} /* evalautes x+1 but returns y */
```

https://stackoverflow.com/questions/18198314/what-is-the-override-keyword-in-c-used-for

###### override keyword

https://stackoverflow.com/questions/18198314/what-is-the-override-keyword-in-c-used-for

###### Explicit keyword for member function

> Explicit Keyword in C++ is **used to mark constructors to not implicitly convert types in C++**. It is optional for constructors that take exactly one argument and works on constructors(with single argument) since those are the only constructors that can be used in type casting.

- Use explicit constructors to ensure the type being converted is not implicitly changing into the class type.

###### constinit

- Ensure a variable is initialised at compile time before linking ensuring global variables being used as **external linkage** specifiers specified by `inline` `extern` or `global` are not used before the **corresponding definition** is generated.

```C++
#ifndef Fuck
#define Fuck
class FileSystem {
public:
	[[nodiscard]] static std::size_t numDisks();
};
#endif

inline constinit FileSystem tfs;
```

```c++
class Directory {
public:
	explicit Directory(int);
};

Directory::Directory(int) {
	std::size_t disks = tfs.numDisks(); // guaranteed to b
}
```

###### Types of casting

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220326162301668.png)

###### enabled_shared_from_this

https://en.cppreference.com/w/cpp/memory/enable_shared_from_this

###### erase remove idiom

https://en.wikipedia.org/wiki/Erase%E2%80%93remove_idiom

###### inline keyword on templates

https://stackoverflow.com/questions/10535667/does-it-make-any-sense-to-use-inline-keyword-with-templates

###### pimpl idiom

- One file contains the declarations with a **pointer** to the **implementat**

###### std::forward

- perfectly forwards the r value reference, l value reference inside a template.

###### std::unique_ptr vs std::shared_ptr

- when using the pimpl idiom
- unique pointers generate a custom delete when there is no default destructor specified within the class its placed in.

###### Default destructors in header interface

- You cant have this with `unique_ptr` as the context requires a complete type as described in the example below.

https://stackoverflow.com/questions/38242200/where-should-a-default-destructor-c11-style-go-header-or-cpp

```c++
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

```c++
#include "A.hpp"

// the previously forward declared class B is now included here
#include "B.hpp"

// we can use the default destructor here as B is
// no longer a partial class
A::~A() = default;  /* the forward declaration is overwritten at this point and thus this is a valid destructor implementation*/
```

###### How bitwise OR represents flags

```c++
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

###### auto decay

```c++
constexpr int i = 42;
const int& ir = i;
uto a = ir; /* takes away the const and & of ir making it just a normal int*/
```

###### alignment

https://stackoverflow.com/questions/17091382/memory-alignment-how-to-use-alignof-alignas

- Alignment is a *restriction* on which *memory positions* some values *first byte* can be stored. 
- Alignment of `16` means the memory addresses that are **multiples** of `16` are the only valid addresses.
- The `alignas` keyword forces alignment to the desired type of powers of `2` only
  - 1,2,4,8,16,32,64,128,....

```c++
#include <cstdlib>
#include <iostream>

int main() {
    alignas(16) int a[4];
    alignas(1024) int b[4]; /* rather than taking up 16 bytes, each one now pads up to 1024 bytes.*/
    printf("%p\n", a);
    printf("%p", b);
}
```

```c++
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
    			/**/
};

int main(){
    std::cout << sizeof(short) << std::endl;
    std::cout << sizeof(short) << std::endl;
    std::cout << sizeof(int) << std::endl; 
    // total size of data members individually is 10 but as there must be padding to ensure proper alignment this changes.
    std::cout << sizeof(Foo2) << std::endl; // size is 12
}
```



###### std::begin vs vector::begin

https://stackoverflow.com/questions/26290316/difference-between-vectorbegin-and-stdbegin

###### Returning by reference

- Do not return a reference to non const local static as it is defined in one place and accessed by multiple locations
- assigning /init a normal variable with a returned reference performs a copy

- object returned by reference must exist after the function returns.
- Return by *address* is returning a **pointer** and thus shares similar caveats.
  - Useful for if we wish to **return** a `nullptr`. 

###### Auto rules

- drops const/volatile and reference qualifiers at the top level.
- does not drop pointers
- auto* must refer to a pointer
- const and auto* vs auto argument order matters

```c++
#include <string>

std::string* getPtr(); // some function that returns a pointer

int main()
{
    const auto ptr1{ getPtr() };  // std::string* const
    auto const ptr2 { getPtr() }; // std::string* const

    const auto* ptr3{ getPtr() }; // const std::string*
    auto* const ptr4{ getPtr() }; // std::string* const

    return 0;
}
```

- Can only apply const twice for auto*

```c++
 const auto const ptr7{ getConstPtr() };  // error: const qualifer can not be applied twice
 const auto* const ptr8{ getConstPtr() }; // const std::string* const
```

###### Virtual inheritance

https://www.sandordargo.com/blog/2020/12/23/virtual-inheritance

https://www.scaler.com/topics/virtual-base-class-in-cpp/ 

https://stackoverflow.com/questions/14163924/virtual-keyword-in-inheritance

###### Ignoring virtualization

```c++
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

###### Prevent deletion of derived from base

https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#c127-a-class-with-a-virtual-function-should-have-a-virtual-or-protected-destructor

if deletion through a pointer to base is not intended to be supported, the destructor should be protected and non-virtual; see [C.35](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#Rc-dtor-virtual).

###### constexpr functions are implicitly inline

> Because constexpr functions may be evaluated at compile-time, the  compiler must be able to see the full definition of the constexpr  function at all points where the function is called.
>
> This means  that a constexpr function called in multiple files needs to have its  definition included into each such file -- which would normally be a  violation of the one-definition rule. To avoid such problems, constexpr  functions are implicitly inline, which makes them exempt from the  one-definition rule.
>
> As a result, constexpr functions are often  defined in header files, so they can be #included into any .cpp file  that requires the full definition.

###### when is a function ran at compile time

> According to the C++ standard, a constexpr function that is eligible for compile-time evaluation *must* be evaluated at compile-time if the return value is used where a  constant expression is required. Otherwise, the compiler is free to  evaluate the function at either compile-time or runtime.

```c++
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

###### consteval

- Function must be evaluated at compile time, otherwise results in an error.

```c++
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

```c++
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
}
```

###### abbreviated function templates

```c++
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

###### runtime vs compiler time

https://stackoverflow.com/questions/846103/runtime-vs-compile-time

###### Implicit deletion

- Data members present that do not have a copy constructor defined on then , implicitly delete the copy constructors on the class they are a part of.

###### Function member discussion

- Compiler generates the code for functions at compilation
- If inline is defined, or if the compiler sees it as suitable then it will execute the function as inline code
- Otherwise it calls the function at runtime
- Alternatively use `constexpr` for a runtime compilation

```c++
#include <iostream>
struct A {
    int x;

    A() : x{f()}{}

    static void f(){
        return 1;
    }
};

int main(){
    /* A constructed and its code for the functions are generated , where
    	static member function is not bound to the objects but rather the class scope
    */
    
    /* It is then constructed at runtime calling the generated functions */
    A a;
}
```

###### pass by reference vs pass by address (pointer vs ref in function parameters)

- Pointers may be null
- Pointers do not allow R values as they are non addressable.

http://www.cplusplus.com/articles/z6vU7k9E/

###### returning a const ref to an auto ref  does not drop the const at low level

```c++
#include <string>

const std::string& getRef(); // some function that returns a const reference

int main()
{
    auto& ref3{ getRef() };       // const std::string& (reference reapplied, low-level const not dropped)
    
    return 0;
}
```

###### auto*

```c++
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

###### classes and other types are free from the One definition rule.

- Types declared in a header file can have multiple definitions amongst the files its included in
- They should be the same definitions otherwise its undefined.

###### user defined deduction guides

```c++
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

###### types of values (pr values, gl values)

https://stackoverflow.com/questions/3601602/what-are-rvalues-lvalues-xvalues-glvalues-and-prvalues

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220519050313660.png)

- `glvalue` $\to$ must evaluate to determine its identity
- `prvalue` $\to$ pure r value , initialise an expression to an object whose resources cannot be reused
- `xvalue` $\to$ is a value that has been deduced to. example is a 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220620231617794.png)

###### void_t explanation for SFINAE

https://riptutorial.com/cplusplus/example/3778/void-t#:~:text=C%2B%2B11,facilitate%20writing%20of%20type%20traits.

```C++
template <class T, class=void>
struct has_foo : std::false_type {};

template <class T>
struct has_foo<T, void_t<decltype(std::declval<T&>().foo())>> : std::true_type {};
```

- So basically if we pass some type `T` without a `foo` function defined this will cause a substitution error and fall back on the false type.
- If it does however we do not need to fall back and thus we instantiate the template with true type to indicate this condition is met where void t just takes this type and converts to void.

###### pass by reference to a constructor

https://stackoverflow.com/questions/4321305/best-form-for-constructors-pass-by-value-or-reference

###### aggregate classes

- class / struct with no
  1. user provided / explicit / inherited constructors
  2. no private / protected non static data members
  3. no virtual functions
  4. no virtual,private protected base classes
- Can also be templates

###### forward declarations

https://stackoverflow.com/questions/4757565/what-are-forward-declarations-in-c

https://stackoverflow.com/questions/3110096/what-is-the-purpose-of-forward-declaration

https://stackoverflow.com/questions/553682/when-can-i-use-a-forward-declaration

###### tag dispatching (C++17)

https://www.fluentcpp.com/2018/04/27/tag-dispatching/

###### typename keyword

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220827140918328.png)

###### non member vs member functions in operator overloading

https://stackoverflow.com/questions/4421706/what-are-the-basic-rules-and-idioms-for-operator-overloading/4421729#4421729

###### static inline functions

https://stackoverflow.com/questions/10876930/should-one-never-use-static-inline-function

###### uses of static keyword

https://stackoverflow.com/questions/15235526/the-static-keyword-and-its-various-uses-in-c

###### static members of class rules.

```c++
class X {
    
    constexpr int x_e {1};
    const int x_c {3};
    static int x; /* not inline, only a declaration , each file that includes must define*/
    int y; /* constructed by X constructor, static variable is not*/
}

class Y {
    inline static int y {1} /* inline definition enforces ODR rule*/
    static void test(){} /* functions are implicitly inline so definitions are allowed*/
}
```

###### unqualfied namesapce calls

- it will be found because unqualified calls will add the namespace of any class argument to the lookup - thus the name argument-dependent lookup.

```c++
namespace mynamespace {
    class MyClass {};
    void swap(MyClass&, MyClass&);
}
mynamespace::MyClass a, b;
swap(a, b);
```

###### mutable lambdas

```c++
#include <iostream>
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

    myInvoke(count);
    myInvoke(count);
    myInvoke(count);

    return 0;
}
```

- These enables the actual lambda to be changed

###### virtual functions in constructors

- dont call them as the base class is created then derived so could be unexpected results.

###### Init summary c/c++

```c++
int main() {
    int i; // garbage stored
    printf("%d", i);
}

```

```c++
int i; // static duration therefore defaults to 0

int main() {
    printf("%d", i);
}

```

```c++
struct A {
    int i; // garbage init
};

int main() {
    struct A a;
    printf("%d", a.i);
}

```

```c++
struct A {
    int i;
} const default_A = {0}; // default struct to C.

void init_A(struct A *ptr) {
    ptr->i = 0;
}

int main() {
    /* helper function */
    struct A a1;
    init_A(&a1);

    /* during definition;
     * Initialize each member, in order. 
     * Any other uninitialized members are implicitly
     * initialized as if they had static storage duration. */
    struct A a2 = {0};

    /* Error! (Well, technically) Initializer lists are 'non-empty' */
    /* struct A a3 = {}; */

    /* ...or use designated initializers if C99 or later */
    struct A a4 = {.i = 0};

    /* default value */
    struct A a5 = default_A;
}

```

- C++

```c++
struct A {
    int i; // default init which is garbage.
};

int main() {
    A a; // trivial thus implicit default constructor which does no work
    std::cout << a.i << std::endl;
}
```

```c++
struct A {
    int i;
};

int main() {
    A a = {.i = 0}; // works as of c++20
    A a = {}; // also works as it default init.
    std::cout << a.i << std::endl;
}

```

- Aggregate types is essentially either a simple C style array or struct that looks like a simple C struct in pre c++11
- No access specifiers, no base classes, no user declared constructors, no virtual functions, aggregate type gets aggregate initialized.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221006173506786.png)

- member class type with constructor > called
- class type with no user provided constructor > recursively value init (value init = empty init)
- built in type > zero init.

---



###### implicit constructor vs default constructor

https://stackoverflow.com/questions/12340257/default-vs-implicit-constructor-in-c

###### function pointer syntax

https://cdecl.org/?q=int+%28*B%28A%2Cint%29%29%28int%29

![img](https://cdn.discordapp.com/attachments/382247936538574853/1027695118217654363/unknown.png)

start from inner test, this is the entry function, marked with no params

params version

```c++
int(*(*(*test(bool))())())() // takes bool
```

- this is a pointer to a function called test taking (bool) which returns a function pointer that points to another function pointer that returns int

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221006214132535.png)

- spirals outwards
- first define intiail name `test` and select return type then make a pointer

```c++
*test(bool) // function pointer taking a bool as input
void (*(*test(bool)))() // add parentheses around it then add a poonter and another pair of brackets
 //this is now a function pointer retunr a function pointer that takes no inputs and returns void
    
// to extend remove void with another pointer and add another pointer layer placing bracket after return type of the other
    
 int(*(*(*test(bool)))())() 
```

###### string view

- does not own the object its looking at
- does not allocate memory when assigned to a string literal so less memory intensive
- prefer over `const std::string& `

###### vector standard implementation

![image](../../images/image-20221013162213223.png)

###### copy / move elision in place construction

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221013174956155.png)

- both are the same elided result which skips the copy or move step into the returned value address passed in under the hood 

###### what is a unions purpose

https://stackoverflow.com/questions/2310483/purpose-of-unions-in-c-and-c

###### using declaration vs using directive

https://stackoverflow.com/questions/16152750/using-directive-vs-using-declaration-swap-in-c

###### argument dependent lookup

https://en.cppreference.com/w/cpp/language/adl

https://stackoverflow.com/questions/8111677/what-is-argument-dependent-lookup-aka-adl-or-koenig-lookup

###### default class member init

https://stackoverflow.com/questions/11594846/default-member-values-best-practice

###### prefer default constructor with no member init list

```c++
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

###### explicit for default constructors with two or more params

https://stackoverflow.com/questions/4467142/why-is-explicit-allowed-for-default-constructors-and-constructors-with-2-or-more

```c++
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

###### copy list init vs direct list init

https://stackoverflow.com/questions/50422129/differences-between-direct-list-initialization-and-copy-list-initialization

- explicit keyword compile time error as shown above

###### user declared vs user defined vs user provided

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221019204353794.png)

https://www.foonathan.net/images/special-member-functions.png

###### implicit compiler declaration charts

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221019204541549.png)

###### r value references to arrays in c++

https://stackoverflow.com/questions/22562187/what-is-the-purpose-of-rvalue-reference-to-an-array-in-c11 

###### naming conventions for classes

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221206163031624.png)

###### types of scope

https://learn.microsoft.com/en-us/cpp/cpp/scope-visual-cpp?view=msvc-170