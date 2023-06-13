# Chapter 1 - Basics

- A variable that has not been given a value is called an **uninitialized variable**. Trying to get the value of an uninitialized variable will result in **undefined behaviour**, which can manifest in any number of ways.

# Chapter 2 -  Functions and Files

- A **declaration** is a statement that tells the compiler about the existence of the identifier. In C++, all definitions serve as declarations. **Pure declarations** are declarations that are not also definitions (such as function prototypes)

- When two identifiers are introduced into the same program in a way that the compiler or linker can’t tell them apart, the compiler or linker will produce a **naming collision**
- **Header guards** prevent the contents of a header from being included more than once into a given code file. They do not prevent the contents of a header from being included into multiple different code files.

# Chapter 4 - Fundamental data types

- Literal suffixes - If the default type of a literal is not as desired, you can change the type of a literal by adding a suffix.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220323000847993.png)

```c++
std::cout << 5; // 5 (no suffix) is type int (by default)
std::cout << 5u; // 5u is type unsigned int
std::cout << 5L; // 5L is type long
```

- The set of values that a specific data type can hold is called its **range**

- **Fixed-width integers** are integers with guaranteed sizes, but they may not exist on all architectures. The fast and least integers are the fastest and smallest integers that are at least some size. std::int8_t and std::uint8_t should generally be avoided, as they tend to behave like chars instead of integers.

# Chapter 5 - Operators

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220323001228489.png)

```c++
#include <iostream>

int main()
{
    int x{ 1 };
    int y{ 2 };

    std::cout << (++x, ++y); // increment x and y, evaluates to the right operand

    return 0;
}
```

- Relational operators can be used to compare floating point numbers. Beware using equality and inequality on floating point numbers.

- precedence chart - https://www.learncpp.com/cpp-tutorial/operator-precedence-and-associativity/#:~:text=4)%20/%202%20%3D%206.-,Table%20of%20operators,-The%20below%20table

# Chapter 6 - Scope , Duration and Linkage

-  Local variables have **automatic storage duration**, meaning they are created at the point of definition and destroyed at the end of the block they are defined in.
- A name declared in a nested block can **shadow** or **name hide** an identically named variable in an outer block. This should be avoided.

```c++
int main(){
    int x{2};
    {
        int x{3}; /* shadows the x above it */
    }
}
```

- An identifier’s **linkage** determines whether other declarations of that name refer to the same object or not
  - Local variables have no linkage
  - Identifiers with **internal linkage** can be seen and used within a single file, but it is not accessible from other files
  - Identifiers with **external linkage** can be seen and used both from the file in which it is defined, and from other code files (via a forward declaration)

- Avoid non-const global variables whenever possible. Const globals are generally seen as acceptable. Use **inline variables** for global constants if your compiler is C++17 capable.

```c++
#ifndef CONSTANTS_H
#define CONSTANTS_H

// define your own namespace to hold constants
namespace constants
{
    inline constexpr double pi { 3.14159 }; // note: now inline constexpr (extern global constant)
    inline constexpr double avogadro { 6.0221413e23 };
    inline constexpr double myGravity { 9.2 }; // m/s^2 -- gravity is light on this planet
    // ... other related constants
}
#endif
```

-  C++ supports **unnamed namespaces**, which implicitly treat all contents of the namespace as if it **had internal linkage**.

```c++
#include <iostream>

namespace // unnamed namespace
{
    void doSomething() // can only be accessed in this file
    {
        std::cout << "v1\n";
    }
}

static void doSomething(); // exact same idea to enforce internal linkage.

int main()
{
    doSomething(); // we can call doSomething() without a namespace prefix

    return 0;
}
```

# Chapter 7 - Control Flow

- **Halts** allow us to terminate our program. **Normal termination** means the program has exited in an expected way (and the `status code` will indicate whether it succeeded or not). 
- **std::exit()** is automatically called at the end of `main`, or it can be called explicitly to terminate the program. It does some cleanup, but does not cleanup any local variables, or unwind the call stack.
- **dangling else** $\to$ ambiguous which if an else is linked too. linked with last unmatched if statement in same block. 
- **sc**

# Chapter 8 - Type Conversion and Function Overloading

- **Implicit type conversion** (also called **automatic type conversion** or **coercion**) is performed whenever one data type is expected, but a different data type is supplied. If the compiler can figure out how to do the conversion between the two types, it will. If it doesn’t know how, then it will fail with a compile error.
- The C++ language defines a number of built-in conversions between its fundamental types (as well as a few conversions for more advanced types) called **standard conversions**. These include numeric promotions, numeric conversions, and arithmetic conversions.

- **numeric promotion** $\to$ conversion of **smaller numeric types** to *larger numeric types* (`int` or `double`).
- A **narrowing conversion** is a numeric conversion that may result in the loss of value or precision.

```c++
double d = 10 / 4; // does integer division, initializes d with value 2.0, narrowing conversion performed
```

- `using` keyword creates an **alias** for some **existing type**.

```c++
using distance_t = double; // define distance_t as an alias for type double
```

- Scope of `alias` same as variable declaration.
- `typedef` with function pointer also.

```c++
typdef long miles_t; // same as using but switched around the idea
using miles_t = long;
```

```c++
typedef int (*fuck_sex)(double, char); // function pointer
using fuck_sex = int(*)(double, char); // same as function pointer above.
```

- When **resolving overloaded functions**, if an exact match isn’t found, the compiler will favour **overloaded functions** that can be matched via **numeric promotions** over those that require **numeric conversions**. 
- When a **function call** is made to **function that has been overloaded**, the compiler will try to match the function call to the appropriate overload based on the arguments used in the function call. This is called **==overload resolution==**
- Auto used as part of **trailing return syntax**
- **ambiguous match** occurs when compiler finds **two or more functions** that can match a function call to an overloaded function and cant determine the best one.

# Chapter 9 - Compound types

- **Return by reference** returns a reference that is bound to the object being returned, which avoids making a copy of the return value.

- If a function returns a reference, and that reference is used to initialize or assign to a non-reference variable, the return value will be copied (as if it had been returned by value).
- **Return by address** works almost identically to return by reference, except a pointer to an object is returned instead of a reference to an object.
- **Type deduction for variables** (via the `auto` keyword) will **drop any reference or const qualifiers** from the **deduced type**. These can be **reapplied** as part of the **variable** **declaration** if desired.
- We can not **reseat** a reference as in make it refer to something else.
- Type deduction for variables (via the `auto` keyword) will  **drop any reference or top-level const qualifiers** from the deduced type.  These can be reapplied as part of the variable declaration if desired.

# Chapter 10

- **program defined type** is a custom type used in our own programs
  - The definition of these is a **type definition**
  - Exempt from ODR.
- **user defined type** = **program defined type**
- **enum** is a compound data type where each value is a **symbolic constant**
  - They are **distinct types** therefore can be **differentiated**
- **unscoped enums** are accessed in same block
- **scope enums** are access via the name using `enum class`.

## Struct aggregate initialisation

- An **==aggregate data type== ** is any type that contains **multiple data members** 
- Some types of **aggregates** allow *members* to have **different types** (*structs* for example)
- *arrays* require the **same data type**.
- To define some **type** as an **aggregate**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220323010237255.png)

### Aggregate initialization of a struct

- Each of these performs a **member wise initialization** 
  - Meaning each member in the struct is **initialized** in order of the **declaration**,

```c++
struct Employee
{
    int id {};
    int age {};
    double wage {};
};

int main()
{
    Employee frank = { 1, 32, 60000.0 }; // copy-list initialization using braced list
    Employee robert ( 3, 45, 62500.0 );  // direct initialization using parenthesized list (C++20)
    Employee joe { 2, 28, 45000.0 };     // list initialization using braced list (preferred)
    Employee joe { 2, 28 }; // joe.wage will be value-initialized to 0.0

    return 0;
}
```

### C++20 Designated initializers

```c++
struct Foo
{
    int a{ };
    int b{ };
    int c{ };
};

int main()
{
    Foo f1{ .a{ 1 }, .c{ 3 } }; // ok: f.a = 1, f.b = 0 (value initialized), f.c = 3
    Foo f2{ .b{ 2 }, .a{ 1 } }; // error: initialization order does not match order of declaration in struct

    return 0;
}
```

### Assignment with an initializer list

```c++
struct Employee
{
    int id {};
    int age {};
    double wage {};
};

int main()
{
    Employee joe { 1, 32, 60000.0 };
    joe = { joe.id, 33, 66000.0 }; // Joe had a birthday and got a raise

    return 0;
}
```

### C++20 Assignment with designated initalizers

```c++
struct Employee
{
    int id {};
    int age {};
    double wage {};
};

int main()
{
    Employee joe { 1, 32, 60000.0 };
    joe = { .id = joe.id, .age = 33, .wage = 66000.0 }; // Joe had a birthday and got a raise

    return 0;
}
```

## Default member initalization

```c++
/* default just sets value if we dont provide aggregrate initalization */
struct Something
{
    int x;       // no initialization value (bad)
    int y {};    // value-initialized by default
    int z { 2 }; // explicit default value
};

int main()
{
    Something s1; // s1.x is uninitialized, s1.y is 0, and s1.z is 2

    return 0;
}
```

### Missing initializers in an initializer list

```c++
struct Something
{
    int x;       // no default initialization value (bad)
    int y {};    // value-initialized by default
    int z { 2 }; // explicit default value
};

int main()
{
    Something s3 {}; // value initialize s3.x, use default values for s3.y and s3.z

    return 0;
}
```

## All Init possibilties

```c++
struct Something
{
    int x;       // no default initialization value (bad)
    int y {};    // value-initialized by default
    int z { 2 }; // explicit default value
};

int main()
{
    Something s1;             // No initializer list: s1.x is uninitialized, s1.y and s1.z use defaults
    Something s2 { 5, 6, 7 }; // Explicit initializers: s2.x, s2.y, and s2.z use explicit values (no default values are used)
    Something s3 {};          // Missing initializers: s3.x is value initialized, s3.y and s3.z use defaults

    return 0;
}
```

---

- Always assign default values to members.

```c++
int numerator { }; // we should use { 0 } here, but for the sake of example we'll use value initialization instead
```

- prefer empty braces if no explicit initializer values.

## Return the struct directly initializing the values

```c++
Point3d getZeroPoint()
{
    // We already specified the type at the function declaration
    // so we don't need to do so here again
    return { 0.0, 0.0, 0.0 }; // return an unnamed Point3d
}
```

## Nested initiliastion

```cpp
#include <iostream>

struct Employee
{
    int id {};
    int age {};
    double wage {};
};

struct Company
{
    int numberOfEmployees {};
    Employee CEO {}; // Employee is a struct within the Company struct
};

int main()
{
    Company myCompany{ 7, { 1, 32, 55000.0 } }; // Nested initialization list to initialize Employee
    std::cout << myCompany.CEO.wage; // print the CEO's wage
}
```

# Chapter 11

- Void pointers are pointers that can point to any type of data. Indirection through them is not possible directly. You can use `static_cast` to convert them back to their original pointer type. It’s up to you to remember what type they originally were.

# Chapter 13 - Operator overloading

- `=` , `[]` , `()` `->` = member function
- unary operator = member function 
- binary operator modifying left operand (`operator+=`) = member function
- binary operator modifying not left operand = function / friend function.

### Typecast overloading

# Chapter 14 - Move Semantics

- `std::move_if_noexcept` will return a movable r-value if the object has a `noexcept` move constructor, otherwise it will return a copyable l-value.  We can use the no except specifier in conjunction with  `std::move_if_noexcept` to use move semantics only when a strong exception guarantee exists (and use copy semantics otherwise).

# Chapter 17 - Inheritance

```cpp
// Inherit from Base publicly
class Pub: public Base
{
};

// Inherit from Base protectedly
class Pro: protected Base
{
};

// Inherit from Base privately
class Pri: private Base
{
};

class Def: Base // Defaults to private inheritance
{
};
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220323015201734.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220323015216338.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220323015223094.png)

## Multiple inheritance

```c++
#include <iostream>

class USBDevice
{
private:
    long m_id {};

public:
    USBDevice(long id)
        : m_id { id }
    {
    }

    long getID() const { return m_id; }
};

class NetworkDevice
{
private:
    long m_id {};

public:
    NetworkDevice(long id)
        : m_id { id }
    {
    }

    long getID() const { return m_id; }
};

class WirelessAdapter: public USBDevice, public NetworkDevice
{
public:
    WirelessAdapter(long usbId, long networkId)
        : USBDevice { usbId }, NetworkDevice { networkId }
    {
    }
};

int main()
{
    WirelessAdapter c54G { 5442, 181742 };
    std::cout << c54G.getID(); // Which getID() do we call?

    return 0;
}
```

# Chapter 18 - Virtual

## Virtual base class

- Insert **virtual keyword** to *share some base class*.

https://www.sandordargo.com/blog/2020/12/23/virtual-inheritance

 ```c++
 class PoweredDevice
 {
 };
 
 class Scanner: virtual public PoweredDevice
 {
 };
 
 class Printer: virtual public PoweredDevice
 {
 };
 
 class Copier: public Scanner, public Printer
 {
 };
 ```

