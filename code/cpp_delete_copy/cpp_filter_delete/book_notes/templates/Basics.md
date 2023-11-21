# The Basics

- Function templates $\to$ parameterized as to **represent** a **family of functions**
  - called on **different types** per se as to be **generic**.

---

## First Template View

### Template Definition

```c++
template<typename T>

/* returns the maximum of two values */
T max (T a, T b)

{
    // if b < a then yield a else yield b 
    return b < a ? a : b;

}
```

- Defines a **family of function** to return the **max** of two values $a$ and $b$. 
- The type $T$ of the **parameters** is left open as a **==template parameter==**. 

`template <comma-seperated-list-of-parameters>` 

---

- **==typename==** introduces a **type parameter**, there may *be alternative parameter types*.
  - Arbitrary type
  - Must support the **template specification** such as above for $<$ 

- $T$ is *convention* as an *identifier* for the **parameter name**.
- The values of $T$ must be **copy able** in order to **be returned**.

---

- Can use **==class==** instead of **typename** also but *not advised* as its **misleading.**
  - Can **not use struct** to define the name.

---

### Using Our Definition

```c++
#include "max1.hpp" /* template defined in external file*/
#include <iostream>
#include <string>

int main()
}
	/* must scope :: to th*/
    int i = 42;
    std::cout << "max(7,i):      " << ::max(7,i) << ’\n’;
    double f1 = 3.4; double f2 = -6.7;
    std::cout << "max(f1,f2):    " << ::max(f1,f2) << ’\n’;
    std::string s1 = "mathematics"; std::string s2 = "math";
    std::cout << "max(s1,s2):    " << ::max(s1,s2) << ’\n’;
}
    std::cout << max_t(str1, str2) << "\n";
}
```

- Templates are **not compiled** to *singular entities* to handle **any type**
  - *Different entities* are generated from the **template** for *every template used*
- `max()` is *compiled* for each of the **three types**

---

- Process of **replacing** the **template parameters** by *concrete types* is **==instantiation==** 
  - Leads to an **instance** of a **template**.

---

- **void** is a *valid* template argument given **code** is *valid*

```c++
template <typename T>
T foo(T*){}
void* vp = nullptr;
foo(vp);
```

---

- Instantiating a **template** for some type **with no support** for *all operations* leads to **compile time errors**.

```c++
std::complex<float1> c1, c2; /* no operator < support */
::max(c1, c2); /* error at compile time */
```

### Two Phase translation

- Deal with **type mismatch** the are **two phases** for *template compilation*

#### Stage 1

- Without **instantiation** at *definition time*, the **template code** is checked for **correctness**
  - This **ignores** the **template parameters**
    - Includes:
      1. Syntax errors.
      2. Unknown names.
      3. Static assertions.

#### Stage 2

At **instantiation time** , template code is **checked** to *ensure* all code *is valid*

- All parts that depend on the **template parameters** are *double checked*.

```c++
template<typename T>
void foo(T t)
{
   undeclared();        // first-phase compile-time error if undeclared() unknown
   undeclared(t);        // second-phase compile-time error if undeclared(T) unknown
   static_assert(sizeof(int) > 10,          // always fails if sizeof(int)<=10
                 "int too small");
   static_assert(sizeof(T) > 10,            //fails if instantiated for T with size <=10
                 "T too small");
}
```

- Names being *checked twice* is a **==two-phase lookup==**. 
- Some compilers do **not perform** the *full checks* of the **first phase**
  - Problems arise until **template** code **is instantiated** at *least once*.

> #### Compiling and linking
>
> - When some **function template** used as such it **triggers** some **instantiation**.
>   - Compiler **must** eventually *see* that **templates** *definition*.
>     - Breaking standard **compile / link** distinction for **ordinary functions** where *declaration* is *sufficient*

- Approach is **implement** the *templates* inside a **header file**.

## Template Argument Deduction

- Template parameters are **determined** by *arguments passed*
  - Pass two **ints** , determine $T$ as **int** 
- $T$ may be **part** of said type such as **using** a *constant references*.

```c++
template<typename T>
T max (T const& a, T const& b)
{
    return b < a ? a : b;
}
```

- Passing in as **int** does **not change**, just it would be a **const reference** within the **definition**.

### Type Conversion during type deduction

- The **automatic conversions** are *limited* during **type deduction**
  1. Declaring **call parameters** by *reference* , the **simple conversions** do *not apply type deduction*
     - Two **arguments** declared with the **same template parameter** $T$ must **match**
  2. By **value** $\to$ trivial **conversions** that *decay* are **supported**
     - Const / volatile are **ignored**
     - references converted to referenced type
     - raw arrays / functions convert to the **corresponding** *pointer type*
     - Two arguments declared with **same template parameter** $T$ , the *decayed types* must **match**

---

```c++
template<typename T>
T max (T a, T b);
…
int i = 32;
int const c = 42;
max(i, c);            // OK: T is deduced as int
max(c, c);            // OK: T is deduced as int
int& ir = i;
max(i, ir);          // OK: T is deduced as int
int arr[4];
foo(&i, arr);        // OK: T is deduced as int*

/* common errors */

max(4, 7.2);        // ERROR: T can be deduced as int or double
std::string s;
foo("hello", s);    //ERROR: T can be deduced as char const[6] or std::string
```

#### Three methods to deal with type errors

##### Cast Arguments so they both match

```c++
max(static_cast<double>(4), 7.2); // ok
```

##### Specify (Qualify) explicitly the type of T

- *Prevent* the **compiler** from **attempting** some *type deduction*.

```C++
max<double>(4, 7.2); // ok
```

##### Specify the parameters could have different types

### Type deduction for Default arguments

- **Type deduction** does *not work* for *default call arguments*

```c++
template<typename T>
void f(T = "");
…
    
f(1);      // OK: deduced T to be int, so that it calls f<int>(1)
f();       // ERROR: cannot deduce T
```

- To support this **case** $\to$ must declare a **default argument** for the **template parameter**

```c++
template<typename T = std::string>
void f(T = "");
…
f();        // OK
```

### Multiple Template parameters

- Function **templates** have **two distinct** sets of parameters.
  1. **==Template parameters==** , which are **declared** in **angles brackets** before the *function* **template** name.
  2. **==Call parameters==**, **declared** in **parentheses** after the **function template name**

---

- $T \max (T \ a, T \ b)$ $\to$ $a$ and $b$ are **called parameters**  
  - You can have **as many template parameters** as you *like*. 
    - Example $\to$ you could define the `max()` template for **call parameters** of *two potentially different types*. 

---

```c++
template<typename T1, typename T2>
T1 max (T1 a, T2 b)
{
    return b < a ? a : b; }
…
auto m = ::max(4, 7.2);        // OK, but type of first argument defines return type
```

- May **appear desirable** to be *pass parameters* of **different types** to the `max()` template.
  - Using **one parameter** as a **return type** 
    - The **argument** for the **other parameter** may get **converted** to **this type**, regardless of **callers intention**.
- The **return type** depends on **call argument order**.
  - max of double vs int may return **one or the other** and thus **cause errors**.

---

