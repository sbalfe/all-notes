# Defining C++ functions

- At **most basic level**
  - function has **parameters** $\to$ has *code* to **manipulate** the **parameters**.
    - Returns a **value**
      - C++ gives **several ways** to *determine* these **three aspects**
- Following **section** $\to$ cover **these parts** of a **C++ function** from **left to right** of the **declaration**.
  - Functions can also **be templated**, left to a **later section**.

## Declaring and defining functions

- must be **defined exactly once**
  - Via **overloading** $\to$ can have the **same name** with *different parameters*

- Code using a function must **access** the **name of the function**
  - Must have the **function definition** (defined **earlier** in the **source file**)
  - Alternatively the **declaration** of the **function** $\to$ called the **function prototype**

- Compiler uses the **prototype** to **type check** that the *calling cod*e that **calls the function** is **using the right type**  

---

- Whichever just **info** for **compiler type checking** the *expression* that *calls function*
  - The **linkers** job to **locate** the **function** in the *library* and *copy the code* into the **executable** or *set up* the **infrastructure** to use the **function** from a **shared library**.

- Inclusion of the **header file** for a **library** does *not mean* that you are **able to use functions** from the **library** as in **standard C++**
  - The **header** file for a *library* does **not mean** you are able to **use the functions** from that library
    - This is as **standard c++** $\to$ the *header file* does **not have information** about the **library** that *contains the function*.

---

- **Visual C++** provides a **pragma** called **comment,** which can be **used** with the **lib option** as a **message** to the **linker to link** with a **specific** library.
-  So #pragma comment(lib, "mylib") in a header file will tell the linker to link with mylib.lib.
-  In general, it is **better** to **use project management tools**, such as **nmake** or **MSBuild**, to ensure that the right libraries are linked in the project.

---

- Most of the **C Runtime library** is like this
  - Function is **compiled** in some **static library** or **dynamic link library**.
    - The **function prototypes** are provided in the **header file**.
- Provided the **library functions** in the **linker command line**
  - Often include the **header** file for the library so the **prototypes** are available to the **compiler**
- As long as the **linker** *knows* about the **library** 
  - You can **type** the **prototype** in **your code** (describe it as **external linkage**) so the *compiler knows* the **function** is **defined elsewhere**
    - This saves you **from including large files** into the **source files**
      - Files will **mostly** have *prototypes* of **functions** that you **will not use**.

---

- Most of **C++** standard library is **implemented** in **header files**
  - Means that these files can **be quite large** 
    - Save **compilation time** by *including* these header files in some **precompiled header**

---

- We have so far just used **one source file** therefore **all the functions** are defined in the **same file** where they are used
  - Have defined the **function** before calling it.
    - That is $\to$ the **function** is *defined above* the *code* that **calls it**.
- Do **not have to define** the *function* before its used.
  - As long as the **function prototype** is *defined* before the **function** is **called**.

```c++
    int mult(int, int); 

    int main() 
    { 
        cout << mult(6, 7) << endl; 
        return 0; 
    } 

	/* after the main function , but as there is a prototype , its fine 
		called forward declaration
	*/
    int mult(int lhs, int rhs) 
    { 
        return lhs * rhs; 
    }
```

- ==Prototypes== do **not require** the **parameter names**
  - This is as the **compiler** only requires the **types**
    - Since **parameter names** are *self documenting* $\to$ often **good idea** to supply **parameter names** to know the **purpose of the function**

---

## Specifying linkage

- In **previous example**, function is **defined** in the **same source file**
  - There is ==internal linkage== 
    - If defined in **another file** $\to$ the **prototype** has ==external linkage== and therefore the **prototype** defined as such

```c++
extern int mult(int, int); // define in another file
```

- The ==extern== keyword is **one of many specifiers** that you can **add** to the **function declaration**
  - Example $\to$ the **static specifier** used on a **prototype** to **indicate** the **function** has *internal linkage*
    - Can only be used **in the current source file**
- In **preceding example** $\to$ appropriate to **mark the function** as ==static== in the **prototype**

```c++
  static int mult(int, int);        // defined in this file
```

- Declare a **function** as ==extern "C"== 
  - This *affects* how the **name** of the function is **stored in the object file**
    - important for **libraries** , covered shortly.

---

## Inlining

- If a **function** calculates some **value** that is **calculated** at **compile time**
  - mark on the **left of the declaration** with **==constexpr==** to *indicate* that **the compiler** can **optimize** the *code* by **computing** the *value* at **compile time**
    - If the **function** value can be **calculated** at the *compile time*
      - Means the **parameters** in the **function call** must be **known at compile time**, therefore *must be literals*
- Function must be **a single line also**
  - If they are **not met** $\to$ the compiler is **free** to *ignore* the **specifier**.

---

- Similar to the **==inline specifier==** 
  - Can be **placed** on the **left** of some **function declaration** as a **suggestion** to the **compiler that** when *other code* calls the function
    - Rather than the **compiler** *inserting* a  **jump** to the **function** in *memory* (and creation of **stack frame**)
      - The **compiler** should **put a copy** of the **actual code** in the *calling function*
- Again the **compiler** is **free** to **ignore the specifier.**

---

## Determining the return type

- Function $\to$ may be **written** to *run* some **routine** and **not return a value**
  - Must specify with ==void==.
    - most cases will, even if just to **indicate** that the **function** has **completed correctly**
      - There is **no requirement** that the **calling** function obtains said **return value** or *does anything with it*
- *Calling* function can **just ignore** the **return value**.

---

- There are **two methods** for specifying the **return type**
  1. Give the **type** *before* the **function name** $\to$ this method used in **most examples so far**
  2. Called the **trailing return type** and requires you **place** *auto* as the **return type** *before the function name* along with `->` *syntax*
     - Give the **actual return type** after the *parameter list*.

```c++
    inline auto mult(int lhs, int rhs) -> int 
    { 
        return lhs * rhs; 
    }
```

- This is **simple enough** to be *inlined*.
  - The **return type** on the **left** is given as **auto**.
    - meaning the **actual return type** is *specified* **after** the **parameter list**.

- The **int** means the **return type** is **int**
  - This *syntax* $\to$ same effect as **using** *int* on the left
    - Useful when some **function** is **templated** and the **return type** may **not be noticeable**.

---

- In this **trivial example** $\to$ can **omit** the *return type* entirely and just use **auto** on the **left** of the **function name**
  - This **syntax means** the *compiler* will **deduce** the **return type**  from the **actual value returned**
- The **compiler** only know what the **return type** is from **function body**
  - Can **not provided** a **prototype** for **such functions**.

---

- If some **function** does **not return** at all
  - Example $\to$ if it **goes** into some **never ending loop** to *poll some value*
    - Mark it with the **C++ attribute** `[[noreturn]]`
      - The **compiler** can use **attribute** to write **more efficient code** as it **knows** that it *does not provide* code to **return a value**

## Naming the function

- The **function names** have the **same rules** for *variables*
  - Must **begin** with a **letter** or an **underscore** and *cannot contain spaces* or **other punctuation characters**
- Following **general principle** of *self documenting code* , should *name* the **function** according to **what it does**
  - There is a **single exception**
    - These are the **special functions** used to **provide overloads** for *operators*  (*mostly punctuation symbols*)
- These functions **have a name** of the form `operatorx` where `x` is the **operator** that you **use in your code**
  - Later explain **implementation** of **operators** with **global functions**

---

- Operators are example of **overloading**
  - Can **overload any function**
    - Use the **same name** but provide **different implementations** and **different parameter types** / different **number of parameters**.

## Function Parameters

- Function can have **no parameters**
  - the function defined with **empty parentheses**
    - A function *definition* must *give* the **type** and **name** of the **parameters** between the **parentheses**
- Most cases $\to$ functions have **fixed number of parameters**
  - Can write **functions** with a **variable number**
    - Can **define** functions with **default values** for *some of the parameters*
      - Thus providing a function that **overloads itself** on the *number of parameters* passed to the **function**
- *Variable argument lists* and **default arguments** later.

## Specifying exceptions

- Functions can be **marked** to **indicate** whether they **throw an exception**
  - More details on this later
- There are **two clear syntaxes** to be **aware of**

---

- *Early versions* of C++ allowed you to use **throw specifier** in **3 ways**
  1. Provide a **comma separated list** of the types of the **exceptions** that **may be thrown**.
  2. Provide an **ellipsis** $(...)$ $\to$ means the **function** *may throw* some **exception**
  3. Provide an **empty pair** of *parentheses* $\to$ means the **function** will **not throw exceptions**
- Syntax looks like this:

```c++
    int calculate(int param) throw(overflow_error) 
    { 
        // do something which potentially may overflow 
    }
```

- The **==throw==** specifier has been **deprecated** in C++11 as of the **ability** to *indicate* the **type of exception** was *not useful*
  - The version of **throw** that indicates **no exception** will be **thrown** is *useful* as it **enables** a **compiler** to **optimize code** by providing **no code infrastructure** to **handle exceptions**
    - C++11 $\to$ retains **this behaviour** with the **==noexcept==** specifer.

```c++
    // C++11 style: 
    int increment(int param) noexcept 
    { 
        // check the parameter and handle overflow appropriately 
    }
```

## Function Body

- After the **return type** , **function name** and **parameters** have *been determined*
  - Must **define** the *body of the function*.
- The **code** for a **function** must **appear** between a **pair of braces {}**
  - If the function returns a value $\to$ the function must have at least one line
- This must return the correct type or a type that can be **implicitly converted** to the **return type** of the **function**
  - As mentioned before $\to$ if the **function is declared** as **returning** ==auto== then the **compiler** must **deduce** the **return type**
    - For this case $\to$ all the **return statements** must return the **same type.**

## Using function parameters.

- Function **called** $\to$ compiler **checks** all the **overloads** of the **function** to *find one that matches* the **parameters** in the **calling code**
  - If there is **not** some **exact match**
    - Then the **standard** and **user defined type conversions** are then *performed*
      - Therefore the **values** provided by the **calling code** may be **different type** from the **parameters**.

- Default $\to$ the **parameters** are passed by **value** and a **copy is made**
  - This means that the **parameters** are treated **locally** inside the function.
    - The **writer of the function** can choose whether to **pass by reference.**, either via a **pointer / reference**
- The *variable* in the **calling code** now able to be **altered** by the **function**
  - This can be **controlled** by making **the parameters** *const*
    - Which case $\to$ the **reason** for **pass by reference** is to **prevent** (potentially costly) **copy being made**
- Built in arrays are **always** passed as a **pointer** to the **first item in the array**
  - The **compiler** will create **temps** when **required**.
    - Example $\to$ when a **parameter** is a ==const== reference and the **calling code** passes **a literal** , a **temporary object** is *created*.
      - Only available to the **code in the function**.

```c++
    void f(const float&); 
    f(1.0);              // OK, temporary float created 
    double d = 2.0; 
    f(d);                // OK, temporary float created
```

## Passing initializer lists

- Can **pass** an **initializer** list as a **parameter** if that **list** can be **converted** to the **type** of the **parameter**
  - Example $\to$ 

```c++
    struct point { int x; int y; }; 

    void set_point(point pt); 

    int main() 
    { 
        point p; 
        p.x = 1; p.y = 1; 
        set_point(p); 
        set_point({ 1, 1 });  
        return 0; 
    }
```

- Defines a **structure** that has **two members**
  - In the `main` function $\to$ a **new instance** of `point` is created on the **stack** and **is init** by *accessing the members directly.*
    - The **instance** is then **passed** to a **function** that has a **point parameter**

---

- As the **parameter** of `set_point` is **pass by value** $\to$ compiler **creates a copy** of the structure on the **stack** of the **function**
  - The **second call** of `set_point` does the same
    - create **temp object** of *point* on the **stack of the function** and *init it* with the **values** in the **initializer list**

## Use of default parameters

- Situations of **one or more parameters** that have values that are so **frequently used** that you treat them as **default parameters**
  - Can alter from the **caller if required**

```c++
    void log_message(const string& msg, bool clear_screen = false) 
    { 
        if (clear_screen) clear_the_screen(); 
        cout << msg << endl; 
    }
```

- Most cases $\to$ this function is **expected to be used** to print a **single message**
  - but occasionally the user may want to have the **screen cleared** 
    - Accommodate this use of the function $\to$ *clear_screen* param has default value of false
      - can choose to set the value if so

```c++
   log_message("first message", true); 
   log_message("second message"); 
   bool user_decision = ask_user(); 
   log_message("third message", user_decision);
```

- Note that the **default values** occur in the **function definition** 
  - And *not* in the **function prototype** , therefore if `log_message` function is **declared** in a **header file** *prototype should be*

```c++
  extern void log_message(const string& msg, bool clear_screen);
```

- The **parameters** that can have **default values** are the **right most**
  - Can **treat each parameter** with **some default value** as *representing* a seperate overload of the **function**
    - Therefore **conceptually** the `log_message` function should be **treated as two functions**

```c++
    extern void log_message(const string& msg, bool clear_screen); 
    extern void log_message(const string& msg); // conceptually
```

- If you define a `log_message` function that has **just** a `const string&` parameter
  - The compiler **wont know** whether to **call** that function or the **version** where `clear_screen` is given a **default value** of **false**.

## Variable number of parameters

- Function with **default parameter** values can be **regarded** as having a **variable number** of *user provided parameters* 
  - Where you **know** at **compile time** the **maximum** number of **parameters** and their **values**, if the caller choose **not to give values**
- C++ $\to$ allows you to **write functions** where there is **less certainty** about the **number of parameters.**
  - Alongside the **values** that are **passed** to the **function**. 

---

- There are **three ways** to have **variable number of parameters**
  1. C-style variable argument lists
  2. variadic templated functions
  3. Initializer list
- Number **2** is addressed in the **chapter** once **templated** functions have **been covered.**

## Initializer lists https://en.cppreference.com/w/cpp/utility/initializer_list

- These so far are treated as **C++ 11 construct**
  - A bit like **built in arrays**
    - When you use the **initializer** list syntax using **braces**
      - The Compiler actually creates an **instance** of the **templated** `initialize_list` class
- If an **initializer** list is used to **initialize** another type (example to init a **vector**)
  - The **compiler** creates an `initializer_list` object with the **values** given **between** the **braces**
    - And the **container object** is **initialized** using the `initialize_list` iterators
- The **ability** to create an `initialize_list` object from a **braced initializer list** can be used by to **give a function** a **variable number of parameters**
  - Albeit the **parameters** must be the **same type**

```c++
    #include <initializer_list> 

    int sum(initializer_list<int> values) 
    { 
        int sum = 0; 
        for (int i : values) sum += i; 
        return sum; 
    } 

    int main() 
    { 
        cout << sum({}) << endl;                       // 0 
        cout << sum({-6, -5, -4, -3, -2, -1}) << endl; // -21 
        cout << sum({10, 20, 30}) << endl;             // 60 
        return 0; 
    }
```

- The **sum** function has a **single parameter** of `initalizer_list<int>`
  - Can only **be initialized** with a **list of integers**
- The `initializer_list` has **very few functions** as it only exists to **give access** to the **values** in the **braced list**
  - *Significantly* $\to$ implements a **size function** that **returns** the **number** of items in the **list**
    - alongside `begin` and `end` functions that **return** a **pointer** to the **first item in the list** and the **position** that is **after list** the **last item**
- These **two functions** are required to **give** *iterator access* to the **list**
  - It enables you to use the **object** with the **ranged for syntax**

>- This is typical in the **C++ Standard Library**. If a container **holds data in a contiguous block of memory**, then **pointer arithmetic** can use the **pointer to the first item** and a **pointer immediately after the last item** to **determine how many items are in the container**.
>- **Incrementing** the **first pointer** gives **sequential access** to every item, and **pointer arithmetic** allows **random access**. 
>  - All containers **implement** a **begin** *and* **end** function to **give access** to the **container iterators**.

- For this example $\to$ the **main** function calls this **function three times**
  - Each time with a **braced init list**
    - return the **sum of the items** within the last
- This **technique** means that **each item** in the **variable** parameter list has to be **the same type**
  - Or some **type** that can be **converted** to the **specified type**
    - You obtain the **same result** if the **parameter** had been a **vector**
      - Difference is that `initializer_list` parameter requires **less initialization**

## Argument lists

- C++ inherits $\to$ from the idea **C** the **idea** of the **argument lists**
  - To do this $\to$ use **ellipses** *syntax* $(...)$ as the **last parameter** $\to$ indicate the **caller** provide the **zero** or **more parameters**
    - Compiler **will** then **check** how the **function** is *called* and **allocate space** on the **stack** for the **extra parameters**
- To *access* the **extra parameters**.
  - Code must include the `<cstdarg>` **header file**.
    - Has **macros** that you can **use** to **extract** the *extra parameters* off **the stack**.

- This is **inherently** *type unsafe*
  - The **compiler** cannot *check* that the **parameters** of that **function** *will get off stack* at **runtime** will be **the same type** as the **parameters** *put* on **the stack** by the **calling code**.
    - Example $\to$ following is an **implementation** of a **function** that will **sum integers:**

```c++
    int sum(int first, ...) 
    { 
        int sum = 0;    
        va_list args; 
        va_start(args, first); 
        int i = first; 
        while (i != -1) 
        { 
            sum += i; 
            i = va_arg(args, int); 
        } 
        va_end(args); 
        return sum; 
    }
```

- The **definition** of the **function** must have at **least one parameter** so that the **macros work**
  - This case $\to$ the **parameter** is called `first`.
    - Important that the **code** *leaves* the **stack** in some **consistent state**.
      - Carried out using **variable** of the `va_list` type.

- The **variable** is **initialized** at the **beginning** of the **function** by calling the `va_start` *macro* 
  - Stack therefore **restored** to its **previous** state at the **end** of the **function** via calling the `va_end` macro

---

- Code in this function **simply iterates** through the **argument list**
  - Maintains a **sum** and the **loop finishes** when the **parameter** has a **value** of **-1**
    - There are **no macros** to *give information* about **how many parameters** there are on the **stack**
- Code assumes the **type** of the **variable** and provide the **desired type** in the `va_arg` macro.
  - For this example $\to$ `va_arg` called, assuming that **every param** on the **stack** is an **integer**

---

- Once **all parameters** are *read off the stack*
  - The **code** calls `va_end` before **returning** the **sum**
    - Function **called like this**

```c++
    cout << sum(-1) << endl;                       // 0 
    cout << sum(-6, -5, -4, -3, -2, -1) << endl;   // -20 !!! 
    cout << sum(10, 20, 30, -1) << endl;           // 60
```

- -1 used to **indicate** the **end of the list**
  - Means it must **sum a zero** *number of parameters*
    - Must pass at **least one parameter**, that is **minus 1**
- Additionally $\to$ the **second line** shows that **you have a problem** if you *passing* a list of **negative numbers**
  - For this case (-1 cannot be parameter).
    - Problem could **be addressed** in this **implementation** by choosing **another marker value**.

---

- Alternative **implementation** could *eschew* use of **marker** for the **end of the list**

  https://docs.microsoft.com/en-us/cpp/c-runtime-library/reference/va-arg-va-copy-va-end-va-start?view=msvc-160

  - Instead use the **first** / **required argument** to **give** the **count** of the **parameters** that *follow*

```c++
    int sum(int count, ...) 
    { 
        int sum = 0; 
        va_list args; 
        va_start(args, count); 
        while(count--) 
        { 
            int i = va_arg(args, int); 
            sum += i; 
        } 
        va_end(args); 
        return sum; 
    }
```

- This time $\to$ the **first value** is the **number of arguments** that follow
  - So the **routine** will **extract** the *correct* number of **integers** off the **stack** and then **sum**
    - Called like this:

```c++
    cout << sum(0) << endl;                         // 0 
    cout << sum(6, -6, -5, -4, -3, -2, -1) << endl; // -21 
    cout << sum(3, 10, 20, 30) << endl;             // 60
```

- There is **no convention** for *how to handle* the **issue of determining** the *number of parameters* have been **passed**

---

- The **Routine** just assumes that **every item** on the stack is an **integer**
  - There is **no information** about this in the **prototype** of the **function**
    - Therefore the **compiler** can **not** do any **type checking** on the **parameters** used to **call the function**
- If the **caller provides** parameter of **different type**
  - The **wrong number of bytes** could be read off the **stack**
    - Making the **results** of **all other calls** to `va_arg` invalid, Considering this:

```c++
  cout << sum(3, 10., 20, 30) << endl;
```

- Easy to **press both** the *comma* and **period keys** at the **same time**
  - This occurred after typing **10**
    - The **period** means **10** is interpreted as a **double** and so the **compiler** puts a **double** value on the **stack**
      - When the **function** reads values off the **stack** with the `va_arg` **macro** reads the **8 byte double** as **two 4 byte ints**
        - This results in a **massive sum** of $1076101140$ 
- This shows the **type unsafe** aspect of the **argument list** 
  - You **can** not **get compiler** to **do type checks** of the **parameters** passed to the **function**

---

- If your **function** has **different types** passed to it **then** you must **implement** some **mechanism** to *determine* what those **parameters are**
  - Good example of **argument lists** is the **C** `printf` function

```c++
int printf(const char* format, ...);
```

- The **required parameter** of this function is a **format string**
  - Importantly $\to$ has an **ordered** list of **variable parameters** and **their types**
- The **format string** provides the **information** that is **not available** via `<cstdarg>` macros
  - The **number ** of **variable parameters** and the **type of each one**
- The **implementation** of the `printf` function will **iterate** through the **format** string and when it **comes across** the *specifier* for **a parameter**
  - A **character sequence** $\%$ read the **expected type** off the **stack** with `va_arg` 
    - Should be **clear** that **C style argument lists** are **not as flexible** as  they **appear** on the **first sight**
      - Moreover $\to$ they are **quite dangerous**.

## Function features

- Functions are **modularized** pieces of **code** defined as **part of your application**
  - Or even, within a **library** 
- If some **function** is **written** by *another vendor* $\to$ important **that your code** calls the **function** in the way **intended** by the **vendor**
  - This means, understanding the **calling convention** used and **how it affects the stack**

## Call stack

- When **calling** a **function**
  - Compiler $\to$ creates a **stack frame** for the **new function call** and it will **push items** on to the **stack**
    - Data **put** on the **stack depends** on *your compiler* and whether the **code** is **compiled** for the **debug / release**
- Generally $\to$ there **will be information** about the **parameters** passed to the **function**, the **return address** (*address after the function call*)
  - Alongside the **automatic variables** allocated in the **function**.

---

- This means $\to$ when you make a **function** call at **runtime**
  - There will be a **memory overhead** and **performance overhead** in *cleaning up* after the **function completes**
- If a function is **inlined**
  - This *overhead* does **not occur** as the **function call** will use the **current stack frame** rather than a **new one**.
    - Clearly **inlined** function should be **small** in **code / memory** used on the **stack**.
- Compiler can **ignore** the ==inline specifier== and **call** the **function** with a **seperate stack frame**.

## Specifying Calling Conventions

- When your code uses **your own functions**
  - Must **pay attention** to *calling conventions* as the **compiler** makes sure the **appropriate** convention is used
    - If you are **writing library code** that can be **used** by **other C++ compilers** or *even other languages*
      - Then the **calling convention** becomes **important**.
- This book is not about **interoperable code**
  - NOT MUCH DEPTH , rather look at **two aspects**
    - *function naming* and *stack maintenance*.

## Using C linkage

https://www.ibm.com/docs/en/i/7.2?topic=linkage-name-mangling-c-only

- When you **give a C++** function **a name**
  - This is the **name** you use to call **the function** in the **C++ code**
- However $\to$ **under** the *covers* $\to$ the **C++ compiler** will *decorate* the **name** with *extra symbols* for the **return type** and the *parameters*
  - So that **overloaded** functions all have **different names**
    - To the **C++ devs** $\to$ called **name mangling**

---

- If you need to **export some function**
  - Through a **shared library** $\to$ called a **DLL** (*dynamic link library*) in *windows*.
    - Use **types** and **names** that **other langauges** can *use*
- To do this $\to$ mark some **function** with **extern** C
  - Means the **function** has **C linkage** $\to$ and the **compiler** will *not use* C++ name **mangling**
    - Should **only use** on functions that will **be used** by the **external code** and *not use* with **functions** that have **return values** and *parameters* that use **C++ custom types**

---

- If some **function** does **return** a **C++** type
  - Compiler only **issue a warning**
    - The reason is that **C** is a **flexible language** and a **C programmer** will be **able to work** out *how* to **turn** the **C++** type to **something usable.** 
      - Poor practice to **abuse like this**

- The `extern "C"` linkage **also** used with **global variables**
  - Can use it on a **single item** (or using braces) on **many items**

## Specifying how the stack is maintained

https://stackoverflow.com/questions/949862/what-are-the-different-calling-conventions-in-c-c-and-what-do-each-mean

EAX register $\to$ store the **return value** for functions.

- Supports **six calling conventions**.
  - that you can **on a function**.

---

- the `__clrcall` $\to$ means that **the function** should be called as **.NET** function
  - Allows you **to write** code that **has mixed native code** and *managed code* 
- C++/CLR (**Microsoft language extensions** to write **C++** in writing **.NET code**) $\to$ beyond **the scope** of this **book**
  - Other five $\to$ indicate **how parameters** are *passed* to a *functions* (on the **stack** or just using **CPU registers**)
    - choose whose **responsibility** it is to **maintain the stack**
- We cover just the **three**
  1. `__cdecl`
  2. `__stdcall`
  3. `__thiscall`

---

https://docs.microsoft.com/en-us/cpp/cpp/thiscall?view=msvc-160

https://docs.microsoft.com/en-us/cpp/cpp/stdcall?view=msvc-160

https://docs.microsoft.com/en-us/cpp/cpp/cdecl?view=msvc-160

- You **will rarely** *explicit* use `__thiscall`
  - Its **calling convention** $\to$ used for **functions defined** as *members of custom types*
    - Indicates that **the function** has some **hidden parameter** that is **pointer** to the *object* that **can be accessed** via the **this keyword** in the **function**.
- More details in the **next chapter**
  - Realize that **such member functions** have *different calling convention*
    - Especially when you **to initialize function pointers**

---

- Default $\to$ C++ **global functions** use the `__cdecl` calling **conventions**
  - The *stack* is **maintained** by the **calling code** 
    - Therefore in the **calling code** $\to$ each call to a `__cdecl` function is **followed** by *code* to **clean up the stack**.
- Makes **each function** call a **little larger**
  - But it **is needed** for *variable argument lists* to be used.
- The `__stdcall` calling convention used by **most windows** *SDK functions*
  - Indicates that the **called function** cleans up the **stack** so there is **no need for such code** to be *generated* in the **calling code**
- Its **important** that the **compiler** knows that a **function** uses `__stdcall` because **otherwise** $\to$ it generates **code** to *clean up a stack frame*
  - Has **already been cleaned** up by the **function**
    - Often see **windows** functions marked with **WINAPI**
      - This is a **typedef** for `__stdcall`. 

## Using recursion

- Most **cases** $\to$ the **memory overhead** of a **call stack** is **unimportant**.
  - When you use **recursion** its possible to **build up** a **long chain of stack frame**.
    - Recursion is when a **function** calls **itself**
      - A **simple example** is a *function* that *calculates* a **factorial**.

```c++
    int factorial(int n) 
    { 
        if (n > 1) return n ∗ factorial(n − 1); 
        return 1; 
    }
```

- Call for 4 $\to$ the **following calls** are **made**:

```c++
    factorial(4) returns 4 * factorial(3) 
        factorial(3) returns 3 * factorial(2) 
            factorial(2) returns 2 * factorial(1) 
                factorial(1) returns 1
```

- Point is that **in recursive function**
  - Must be at **least one way** to *leave some function* with **out recursion**
- For this case $\to$ its when **factorial** is called with a **parameter** of $1$ 
  - Practice $\to$ a **function** like this should be **marked inline** to *avoid any stack frames at all*

---

## Overloading Functions

- Can **have several functions** each with the **same name**
  - The **parameter list** is different however in each one (the **number of parameters** / **type** of *parameters*)
    - This is ==**overloading**== the *function name.*

---

- With **such a function** is *called* 
  - The **compiler** will *attempt* to **find the function** that *best fits* the **parameters** *provided*
    - If there is **not a suitable function** $\to$ the **compiler** will **attempt** to **convert the parameters** to see if some function with **them types exists**
- Start with **trivial conversions**
  1. The **array name** to a **pointer**
  2. type to **const type**
- If this fails $\to$ tries to **promote** the **type** (*bool* to *int*).
- If this fails $\to$ compiler **tries standard conversions** (*reference* to *type*)
  - If **such conversions** result in **more than one possible** candidate
    - compiler **issues an error** that the **function** call is **ambiguous**

## Functions and scope

- The **compiler** takes the **scope** of the **function** into *account* when for **looking** for **some suitable function**
  - Can **not define** a **function** **within** some **other function**
    - You *can* provide a **function prototype** *within* the *scope* of **some other function** however.

```c++
    void f(int i)    { /*does something*/ } 
    void f(double d) { /*does something*/ } 

    int main() 
    { 
        void f(double d); 
        f(1); 
        return 0; 
    }
```

- The function **f** $\to$ *overloaded* with **one version** that takes an **int**
  - The other takes an **double**.
- Calling $f(1)$ $\to$ the **compiler** would take the **first version** of the **function**
  - In **main** there is a **prototype** for the **version** that takes a **double**
    - Ints can be **converted** to a **double** *without loss*

- The **prototype** in the **same scope** as the **function call**
  - Therefore in this code $\to$ the **compiler** call the **version** that **takes the double**
    - This just **hides** the *version* with an **int parameter**.

## Deleted functions

- There is a **more formal** way to **hide functions** rather than **using** the **scope**
  - C++ $\to$ attempts **to explicitly** *convert* the **built in types**

```c++
void f(int i);
```

- Can **call** this with an **int**
  - Or anything that can **be converted** to an **int**

```c++
f(1);
f('c'); // convert char to ascii value
f(1.0); // warning of conversion
```

- *Second case* $\to$ a **char** is an **integer**
  - Promoted to an **int** and the **function** called.
- *third case* $\to$ the **compiler issue** a **warning** that the *conversion* may cause **loss of data**
  - Only a **warning** of course.

---

- To ==prevent implicit conversions== $\to$ can **delete** the **functions**  that you **do not** want the **callers** to *use*
  - Provide a **prototype** and use the **syntax** `= delete` 

```c++
    void f(double) = delete; 

    void g() 
    { 
        f(1);   // compiles 
        f(1.0); // C2280: attempting to reference a deleted function 
    }
```

- When the **code attempts** to *call the function* with a **char** or a **double** / **float** (IMPLICT DOUBLE CONVERSION)
  - Compiler issues an **error**.

## Passing by value and passing by reference

- Default $\to$ compiler passes by **value** (*copy* is **made**)
  - Passing a **custom type** $\to$ the **copy constructor** is *called* to **create new object**
- Passing a **pointer** to an **object** of some **built in type** / **custom**
  - The **pointer** passed by **value** $\to$ a **new pointer** created on the **function stack** for the **parameter**
    - Initialized with the **memory address** that is **passed** to the **function**
      - In the **function** $\to$ changing the **pointer** to some **other memory** (useful for using **pointer arithmetic** on **that pointer**)

- The **data** pointed to by said pointer $\to$ passed **by reference**
  - The *data* remains **outside** of **the function**
    - The **function** can *use* the *pointer* to **change** the *data*
- Similarly $\to$ using a **reference** on a **parameter** $\to$ means the **object** is *passed* by **reference**
  - Using a **const** on a **pointer**  or **reference** *parameter*
    - Then this **will affect** whether the **function** can **change the data** that is **pointed** to or **referenced**.

---

- May **want** to **return several** *values* from a **function** 
  - may choose to **use return value** of the **function** to **indicate** if the **function** *executed correctly*
    - Make **one of the parameters** an **==out parameter==** 
      - Either a **pointer** or a **reference** to an **object** or *container* that **function** *alters*.

```c++
    // don't allow any more than 100 items 
    bool get_items(int count, vector<int>& values) 
    { 
        if (count > 100) return false; 
        for (int i = 0; i < count; ++i) 
        { 
            values.push_back(i); 
        } 
        return true; 
    }
```

- To **call** this function
  - Must create a **vector** *object* and **pass** it to the **function**

```c++
    vector<int> items {}; 
    get_items(10, items); 
    for(int i : items) cout << i << ' '; 
    cout << endl
```

- The **values parameter** is a **reference**
  - Therefore $\to$ when your **get_values** calls **push_back** to *insert a value* in the **values** *container*
    - It actually **inserting** that *value* into the **items container**.

---

- If some **out parameter** is *passed* via a **pointer**
  - Must look at **pointer declaration**.
    - The single ***** means the **variable** is a **pointer**.
      - **two** means its a **pointer to a pointer**
- Following function returns an **int** through an **out parameter**

```c++
    bool get_datum(/*out*/ int *pi);
```

- Code *called* as **such**

```c++
    int value = 0; 
    if (get_datum(&value)) { cout << "value is " << value << endl; } 
    else                   { cout << "cannot get the value" << endl;}
```

- This **pattern** of **returning** a **value** indicating *success* is **frequently used**
  - Particularly with **code** that *accesses* *data* across **process / machine boundaries**.
- Function **return value** gives **detailed information** on why the **call failed**
  - (*no network access* / *invalid security credentials* etc)
    - Indicates the **data** in the **out parameters** must be **discarded**

---

- If ==out parameter== has a **double ***
  - Means the **return value** is *in itself* a **pointer**
    - Either a **single value** or to some **array**

```c++
    bool get_data(/*in/out*/ int *psize, /*out*/ int **pi);
```

- For this $\to$ can **pass** in the **size of the buffer** using the **first parameter**
  - For **return** $\to$ receive the **actual size** of the **buffer** via *this parameter* (*in / out*) and a **pointer** to the **buffer** within the **second parameter**

```c++
    int size = 10; 
    int *buffer = nullptr; 
    if (get_data(size, &buffer)) 
    { 
        for (int i = 0; i < size; ++i) 
        { 
            cout << buffer[i] << endl; 
        } 
        //delete [] buffer; 
    }
```

- Any **function** that **returns a memory** *buffer* must **document** who has the **responsibility** of *deallocating the memory*
  - Most cases $\to$ often the **caller** / assumed in the code often.

## Designing functions

- Functions often **act upon global data**
  - or *data* passed in **by the caller**
    - Important that **when the function completes** $\to$ leaves the **data** in a **consistent state**
- Equally $\to$ important that **the function** can **make assumptions** about the **data** before **it accesses it**

## Pre and post conditions

- Function often **alters some data**
  1. Values **passed** into the **function**
  2. data **returned** by the **function**
  3. **Global data**
- Important when **designing** a *function*
  - that you *determine* what **data** is *accessed* and **changed** and that the **rules are documented**

---

- A *function* has some **pre-conditions**
  - assumptions on the **data** it *will use*.
    - Example $\to$ if a *function* is passed some **filename** $\to$ with **intention** that *the function* will **extract some data** from the *file*
      - Whose responsibility to **check that the file exists** $\to$ make is the **functions responsibility** 
        - The *first few lines* check its a **valid path** to some **file** and *call operating system functions* to *check the file exists*.
- If there are **many functions** on the **same file**
  - You can **replicate this code** therefore maybe **put** responsibility in the **calling code**.
    - Such *actions* are **expensive** $\to$ important to **avoid both** *calling code / function* to **perform these checks**.

---

- Chapter 7 $\to$ describe how **to add debugging code**
  - These are **==asserts==** that you **place** in **your functions** to *check  values* of the **parameters** to *make sure* the **calling code** is following **pre conditions** set
    - Asserts are **defined** using *conditional compilation* therefore only **appear** in **debug builds** (C++ code compiled **with debugging information**).
- ==Release builds== (completed code **delivered** to the **end user**)
  - Often **conditionally compile asserts** away 
    - Making **code faster** 
      - if thorough **testing** $\to$ assured the **pre conditions are met**.

---

- Must **document** the **post conditions** of your **function**
  - This is the **assumption** on the **data returned** by the **function** (through the **function return value** / **out parameters** or parameters **passed by the reference**).

---

- *Post conditions* $\to$ the **assumptions** the **calling code** will *make*
  - Example $\to$ may **return a signed integer** where **the function** is *meant* to **return a positive value**
    - But a **negative value** to **indicate an error**

- Functions that **return pointers** often return **nullptr** if **failure**
  - Both cases $\to$ the **calling code** knows **it needs** to **check** the *return value* and **only use ** if its **either positive** or *not nullptr*.

---

## Using invariants

https://www.cplusplus.com/reference/ios/ios_base/flags/

- Be **careful** to **document** how a **function** uses *data external* to the **function**
  - If the **intention** of *some function* is to **change external data**
    - Must **document** what the **function will do**.
- If you **dont explicitly** *document* what the **function** does to **external data**
  - must **ensure** when the **function finishes** $\to$ such data is **left untouched**.
    - Reason $\to$ that **calling code** only assume what **you said** in the **documentation** and the **side effect** of *changing global* data **may cause problems**

- Sometimes **necessary** to *store* *state* of the **global data** and **return** the **item back** to that **state** before the **function returns**.

---

- Example $\to$ the **cout** object
  - The **cout** is *global* to the **application** $\to$ can be **changed** through **manipulators** to make it **interpret** *numeric values* in *certain ways*
    - Changing it in a **function (inserting hex)**
      - Change will **remain** when the **cout** object used **outside the function**

---

- Create a function called read16 that reads 16 bytes from a file and prints the values out to the console both in hexadecimal form and interpreted as an ASCII character:

```c++
    int read16(ifstream& stm) 
    { 
        if (stm.eof()) return -1;  

        int flags = cout.flags(); 
        cout << hex; 
        string line; 

        // code that changes the line variable 

        cout.setf(flags); 
        return line.length(); 
    }
```

- Stores the **state** of the **cout** object in some **temporary variable** of *flags*
  - read16 **function** can *change* the *cout* object in **any way necessary**
    - As we **have stored**  state $\to$ means the **object** can be **restored** to **original state** *before returning*.

## Function pointers

- When some **application** is **run**
  - The **functions** it will **call** exists in **memory**  somewhere
    - Means you can get the **address** of a **function**

- C++ allows you to use **the function** *call operator* (parentheses ())
  - To call a **function** through the **function pointer**.

## Remembering the parentheses

- A **simple example** of *how* function **pointers** cause **difficulty** in *noticing bugs* in **the code**
  - A **global function** called **get_status** *performs* various **validations** actions to **determine** if the **state** of the **system** is **valid**
    - Function returns a **value of zero** to mean the **system state** is *valid* and *values* over **zero** are **error codes**

```c++
    // values over zero are error codes 
    int get_status() 
    { 
        int status = 0;  
        // code that checks the state of data is valid 
        return status; 
    }
```

- Code **can be called as such**

```c++
    if (get_status > 0) 
    { 
        cout << "system state is invalid" << endl; 
    }
```

- This is **an error** as the developer has **missed** the **()**
  - Therefore the **compiler** does **not treat as function call**
    - it rather treats it as a **test** of the **memory address** of the **function**
      - As the **function** will *never be located* at a **memory address of zero** $\to$ comparison **is always true**
        - Therefore the **message** will **be printed** even if the **systems state** is **valid**.

## Declaring function pointers

https://www.cprogramming.com/tutorial/function-pointers.html

- Last section $\to$ **highlights** how *easy* to **get address** of a **function**
  - Literally just **using** the **name** of the **function** without *parentheses*

```c++
    void *pv = get_status;
```

- Pointer **pv** is only **mild interest**
  - You **know** now where the **function** is stored in **memory**
    - To **print** the address $\to$ must still **cast it an integer**.
- To **make** the **pointer useful** 
  - Must be able to **declare** a **pointer** for which the **function** can **be called**

---

- Refer to **function prototype**

```c++
int get_status()
```

- The ==function pointer== must be **able** to *call the function* passing **no parameters** and *expecting* a **return value** of an **integer**
  - The **function pointer** declared as **such**

```c++
    int (*fn)() = get_status;
```

- The ***** indicates the **variable**  **fn** is a **pointer**
  - This **binds** to the **left** $\to$ *without* the parameter surrounding the ***fn** $\to$ the **compiler** would **interpret** this to **mean** the *declaration* is an **int*** pointer
    - The **rest** of the **declaration** indicates **how the function** *pointer* is **called**
      - Taking **no parameters** and *returning* an **int**.

---

- Calling **through a function** *pointer* is **simple**
  - Give the **name** of the **pointer** where you **would normally give** the *name of the function*

```c++
    int error_value = fn();
```

- Importance of the **parentheses** 
  - Indicates the **function** at **address** held in the **function pointers** $fn$ is *called*

---

- **Function** **pointers** $\to$ make **code look rather cluttered**
  - Especially when **you use** them to **point** to **==templated functions==** 
    - Often *code* will **define an alias**.

```c++ 
    using pf1 = int(*)();
    typedef int(*pf2)();
```

- These **two lines** declare **aliases** for the **type** of the *function* pointer needed to **call** the `get_status` function
  - Both **valid** , however the *using* **the version** is *more readable* since it **is clear** that **pf1** is the **alias being defined**
    - See why $\to$ consider **this alias** 

```c++
    typedef bool(*MyPtr)(MyType*, MyType*);
```

- The **type** *alias* is called **MyPtr**
  - And it is to a **function** that **returns** a *bool* that takes **two** *MyType* *pointers*
    - Much **clearer** with **using**.

```c++
    using MyPtr = bool(*)(MyType*, MyType*);
```

- **Tell tale** sign $\to$ is the ***** 
  - Indicates the **type** is a **function pointer** because you are **using the parenthesis** to *break associativity* of the *****
    - Can **read outwards** to *see the prototypes* of the **function**
      - To the *left* to **see the return type**
      - To the **right** to **get the parameter list**
- Once you **have** *declared* an **alias**
  - Create a **pointer** to a **function** and **call it**.

```c++
    using two_ints = void (*)(int, int); 

    void do_something(int l, int r){/* some code */} 

    void caller() 
    { 
        two_ints fn = do_something; 
        fn(42, 99); 
    }
```

- As the **two_ints** alias is **declared as a pointer**
  - Do **not use** a ***** when *declaring* a **variable** of **this type**

## Using function pointers

- *function pointer* is **just a pointer**
  - This *means* that **you can** use it as a **variable** $\to$ you *can* return it **from a function** or **pass as a parameter**
    - Example $\to$ may **code** that performs **lengthy routine** and want to **provide** *feedback* during the **routine**
- Make **this flexible** $\to$ could *define* to take a **callback pointer** and **periodically** in the **routine call** to **indicate** progress.

```c++
    using callback = void(*)(const string&); 

    void big_routine(int loop_count, const callback progress) 
    { 
        for (int i = 0; i < loop_count; ++i) 
        { 
            if (i % 100 == 0) 
            { 
                string msg("loop "); 
                 msg += to_string(i); 
                 progress(msg); 
            } 
            // routine 
        } 
    }
```

- Here `big_routine` has a **function** *pointer parameter* called **progress**
  - The **function** has a **loop** that *will* be **many times** and **every one hundredth loop** it *calls* the **callback**
- Passing a **string** gives **information** on the **about the progress**

> - *Note that the* string *class defines a* += *operator that can be used to append a string to the end of the* string *in the variable and the* <string> *header file defines a function called* to_string *that is overloaded for each of the built-in types to return a* string *formatted with the value of the function parameter.*

- Function **declares** the **function pointer** as *const* *merely* so that **compiler knows** that *the function pointer* should **not be changed** to a **pointer** to *another function* in **this function**
  - Code *called as such*

```c++
    void monitor(const string& msg) 
    { 
        cout << msg << endl; 
    } 

    int main() 
    { 
        big_routine(1000, monitor); 
        return 0; 
    }
```

- The **monitor** function has the **same prototype** as described by the **callback** *function pointer*
  - For example $\to$ the *function parameter* was **==string&==** and not **==const string&==**
    - Then the **code** will *not compile*.
- The `big_routine` function *is then called*
  - Passing a **pointer** to the **monitor** function as the **second parameter**
- Passing **callback functions** to *library code*
  - Must **pay attention** to the **calling convention** of the *function pointer*
    - Example $\to$ if you pass a **function pointer** to a **windows function** (e.g. such as **EnumWindows**)
      - Must point to a **function** declared the **__stdcall** calling **convention**.
- C++ standards uses **another technique** to *call functions* defined at **runtime**
  - Which is **functors**.

## Templated functions

- When you **write library code**
  - Often write **several functions** $\to$ differ only **between types** that are **passed** to the **functions**
    - The **routine** action is the **same** $\to$ its just the **type that have changed**
- C++ provide **templates** $\to$ to *allow* you **to write more generic code**
  - write the **routine** using a **generic type** at *compile time*

- Compile generates a **function** with **appropriate types** 

  - The **templated function** is *marked* as such using **==template==** keyword and a **list of parameters** in *angle brackets* $<>$ 
    - Give **placeholders** for the **types** that are **used**.

  >  Important to **understand** that these **template parameters** are *types* and *refer* to the **types of parameters** (return value of the **function**) 
  >
  > that will **be replaced** with the **actual types** used by **calling the functions**

- They are **not parameters** of the **function**
  - Do *not normally provide* them when **you call the function**.

---

- Best to **explain** template functions with an **example**
  - A simple **maximum function** written as such:

```c++
    int maximum(int lhs, int rhs) 
    { 
        return (lhs > rhs) ? lhs : rhs; 
    }
```

- Call with **other integer types**
  -  Smaller **types** (*short / char / bool*) are **promoted** an **int**
    - Values of **larger types** such as **long long** will be **truncated**.
- Similarly $\to$ a **variables** of **==unsigned==** types **will be converted** to the *signed int* which **could cause problems**
  - Consider **this call of the function**.

```c++
    unsigned int s1 = 0xffffffff, s2 = 0x7fffffff; 
    unsigned int result = maximum(s1, s2);
```

- Whats the **value** of the **result** *variable*: $s1$ / $s2$ $\to$ its clearly **s2**
  - Reason that **both values** are *converted* to **signed int** and when *converted* to a **signed type** *s1*
    - Is of value **of** $-1$ and **s2** will be a **value** of **2147483647**

- To **handle unsigned types**
  - Must **overload** the **function** and *write* a **version** for *signed* / *unsigned* **integers.**

```c++
    int maximum(int lhs, int rhs) 
    { 
        return (lhs > rhs) ? lhs : rhs; 
    } 

    unsigned maximum(unsigned lhs, unsigned rhs) 
    { 
        return (lhs > rhs) ? lhs : rhs; 
    }
```

- The **routine is the same**
  - However the **types** have **changed**
    - There is **another issue** $\to$ what if the **caller mixes** types
      - Does this **make sense**.

```c++
    int i = maximum(true, 100.99);
```

- This **code** will **compile** because a *bool* and a *double* can be **converted** to an **int**
  - The **first** load will be **called**
    - This is **complete nonsense** $\to$ better if the **compile** *caught this error*.

## Defining templates

- Return to **two versions** of the **maximum function** 
  - The **routine** is the **same** for **both**
    - All that's **changed** is the **type**
- If you **had** a **generic type** $\to$ call it $T$ 
  - Where $T$ can be **any type** that *implements* an **operator>**
    - Routine could be **described** by this **pseudocode** 

```c++
    T maximum(T lhs, T rhs) 
    { 
        return (lhs > rhs) ? lhs : rhs; 
    }
```

- This **wont compile** as we have **not defined** the **type T**
  - Templates enable you to **tell the compile** that the **code** uses a **type** and **will** be **determined** from the **parameter** passed to the **function**
    - Following **code compiles**

```c++
    T maximum(T lhs, T rhs) 
    { 
        return (lhs > rhs) ? lhs : rhs; 
    }
```

- Specifies the **type** that will be **used** using the **==typename==** identifier
  - The type **T** is a **placeholder**
    -  Can **use** *any name* you like as **long** as its **not a name** used **elsewhere** at the **scope**
      - Must be in the **parameter** list of the **function**.
- Can use **class** instead of **typename**
  - but the **meaning** is the **same**.

---

- Call **this** function , passing **values** of *any type*
  - The **compile** *creates* the code for **that type**
    - Calling the **operator>** for *that type*.

> Important to **realize** that $\to$ first time the **compiler comes** across a **templated** function
>
> - Create a **version** of the **function** for the **specified type**
>   - If you **call** the **templated function** for *several different types*
>     - The *compiler* create / instantiate or a **specialized function** for *each types*

-   Definition of **this template** $\to$ **indicates** that *only one type is used*
  - only call it with **same type** parameters.

```c++
    int i = maximum(1, 100);
    double d = maximum(1.0, 100.0);
    bool b = maximum(true, false);
```

- Give **expected results**
- This will not compile: 

``` c++
    int i = maximum(true, 100.99);
```

- The **template parameters** $\to$ only gives a **single type**
  - If you wish to **define a function** with *different types*:

```C++
    template<typename T, typename U> 
    T maximum(T lhs, U rhs) 
    { 
        return (lhs > rhs) ? lhs : rhs; 
    }
```

> This is **done to illustrate** how *templates work*
>
> - often ==doesn't make sense== to define some function that takes two different types

---

- This version is **for two different types**
  - the **template declaration** mentions two types
    - These are used for the parameters.
- The **function** returns **T**
  - The **type** of the **first parameter**, function can be **called as such**

```c++
    cout << maximum(false, 100.99) << endl; // 1 
    cout << maximum(100.99, false) << endl; // 100.99
```

- output from first is **1**
  - Result of **second is 100.99**
- This is strange as they appear to be the same, but 
  - The **return value** is always the **T** as in the **LHS** therefore just returns the **lefthand side value**

- if the **template** version of ==maximum== changed to **return U** , reverse the values.

---

> Calling a **template function**
>
> - Do **not** have to **give the types** of the *template parameters* as the *compiler* can **deduce them**

- Important to note that this applies only **to parameters**
  - The **return type** $\to$ is *not determined* by the **type of the variable** 
    - The *caller* *assigns* to the **function value** as the function can be **called** with **no return value**.

---

- The **compiler** will *deduce* the *template parameters* from **how the function** is *called*
  - You can **explicitly** provide the **types** in the *called function* to **call a specific version** of the *function*
    - And if **necessary** $\to$ get the **compiler** to *perform implicit conversions*

```c++
    // call template<typename T> maximum(T,T); 
    int i = maximum<int>(false, 100.99);
```

- The code *calls the version* of **maximum** that has **two int parameters** 
  - And then returns **int** therefore the return value **is 100**, 100.99 > integer.

## Using template parameter values

- The **template** defined so **far** has had the **types** as the *parameters*
  - Can provide **integer values also**
    - Following is **contrived example** of this.

```c++
    template<int size, typename T> 
    T* init(T t) 
    { 
        T* arr = new T[size]; 
        for (int i = 0; i < size; ++i) arr[i] = t; 
        return arr; 
    }
```

- There are **two template parameters**
  - The *second parameters* $\to$ provides the **name** of *type* where $T$ is a **placeholder** for the **type** of the **parameter** of the **function**
- First parameter $\to$ is **like a function** *parameters* as its used in a **similar way**
  - ==size== used **locally** within the function (as in its **read only**) 

- The **function parameter** is $T$ and therefore the **compiler** can *deduce* the **second template** parameter from the **function call**
  - It *cannot* however **deduce** the **parameter** therefore you *must provide a value* in the **call**
    - This is the **example** of *calling* this template function for an **integer** for $T$ and **a value** of $10$ for **size**.

```c++
   
	/* 
		calls function with 10 as the template paramter and 42 as function parameter
		
		42 is an int, the init function creates an int array with ten members init to 42
		
	*/
	int *i10 = init<10>(42); 
    for (int i = 0; i < 10; ++i) cout << i10[i] << ' '; 
    cout << endl; 
    delete [] i10;
```

- The **compiler** can *deduce* the **int** as the **second parameter**
  - This code however could have **been called** the function with `init<10, int> (42)` to **explicitly** indicate that you **require** an *int array*.

---

- The *non-type parameters* must be **constant** at *compile time*
  - Value can be **integral** (includes **ENUM**) however *not floating point*.
- Can use an **arrays** as *integers*
  - These will available through the **template parameter** as a *pointer*.

---

- Most *cases* the *compiler* can **not deduce** the *value parameter* 
  - It **can** if the **value** is *defined* as the **size of an array** $\to$ 
    - This could be used to **make it appear** a function can **determine** the *size* of a **built in array**
      - It of course *cannot* as the **compiler** creates a **version** of the **function** for *each size*.

```c++
    template<typename T, int N> void print_array(T (&arr)[N]) 
    { 
        for (int i = 0; i < N; ++i) 
        { 
            cout << arr[i] << endl; 
        } 
    }
```

- There are **two template parameters** present here
  1. **type **of **array**
  2. The **size** of the **array** 
- This *looks odd* $\to$ but its **just** a *built* in *array* being **passed by a reference** 
  - If the **parentheses** are *not used* then the **parameter** is `T& arr[N]` 
    - An **N sized array** of *references* to an **object** of *type T* $\to$ which we *dont want*.
- We want an **N-sized** *built in array objects of type* **T**
  - Function called **as such**.

```c++
    int squares[] = { 1, 4, 9, 16, 25 }; 
    print_array(squares);
```

- The interesting thing on this $\to$ the **preceding code** is that *the compiler* sees that there **are five items** in the **init list**

```c++
   print_array<int,5>(squares);
```

- The **compiler** will **instantiate** this function for *every combination* of **T** and **N** that the **code calls**
  - If the **template** function has a **large** ammount of code
    - This **may be an issue**.
- Work around to this is a **helper function**

```c++
    template<typename T> void print_array(T* arr, int size) 
    { 
        for (int i = 0; i < size; ++i) 
        { 
            cout << arr[i] << endl; 
        } 
    } 

    template<typename T, int N> inline void print_array(T (&arr)[N]) 
    { 
        print_array(arr, N); 
    }
```

- This does **two things**

  1. A version of **print_array** that takes a **pointer** and the **number of items** the pointer points to.
     - This mean $\to$ the **size parameter** is *determined* at **runtime**
       - Therefore **versions** of *this function* are only **instantiated** at *compile time* for the **types** of **the arrays used**
         - Not for **both** the *type* and **array size**.

  2. The **function** that is *templated* with the **size** of the **array** is *declared as **inline*** 
     - Calls the **first version** of the **function**
       - Although there will be a **version** of this *for every combination* of **type / array size**
         - The **instantiation** will be **inline** rather than a **complete function**.

---

## Specialized templates

- May have **routine** that works for **most types**
  - Alongside a **candidate** for a *templated function* 
    - You **may identify** that some types require a **different routine**

- Write a **==specialized template function==**  $\to$ A function that is **used** for some **specific type** and the **compiler** uses **this code**
  - When a **caller** uses **types** that *fit* this **specialization** 
- Return size of a type.

```c++
    template <typename T> int number_of_bytes(T t) 
    { 
        return sizeof(T); 
    }
```

- Works for **most built in types**
  - If you **call** it **with a pointer**  $\to$ obtain the **size** of the *pointer*, not what it points to.
- Therefore $\to$ `number_of_bytes("X")` $\to$ will **return** $4$ (on a **32 bit system**)
  - rather than **2** for *size* of the **char array**. 
- May decide you **want** some *specialization* for **char*** pointers that use **C** *function* of *strlen*
  - To **count** the **number of characters** in the **string** until the **NUL** characters.
    - Require a **similar prototype** to the **templated function**, replacing the **template parameter** with the *actual type*
      - Since the **template parameter** is *not required*  $\to$ not required to **specific** type to the **function name**

```c++
  template<> int number_of_bytes<const char *>(const char *str) 
    { 
        return strlen(str) + 1; 
    }
```

- When calling `number_of_bytes("X")` the **specialization** will **be called** and return a **value** of **2**
  - Earlier $\to$ defined a **templated function** to *return* a **maximum** of the **two parameters** of *same type*. 

```c++
    template<typename T> 
    T maximum(T lhs, T rhs) 
    { 
        return (lhs > rhs) ? lhs : rhs; 
    }
```

- With **specialization** $\to$ can **write versions** for *types* that are **not compared** using the **> operator**
  - Since it **makes no sense** to *find* the *max* of **two booleans** 
    - Can *delete* the **specialization** for **bool**.

```c++
template<> bool maximum<bool>(bool lhs, bool rhs) = delete;
```

- This **generates** a **error** if the code calls maximum with **bool parameters** in compilation.

---

## Variadic templates

- This is a template when there is a **variable** *number* of *template parameters*
  - The **syntax** is *similar* to **variable arguments** to some functions (use **ellipses**)
    - However $\to$ use them **on the left** of the **argument** in the *parameter list* $\to$ declares it a **parameter pack**

```c++
    template<typename T, typename... Arguments>  
    void func(T t, Arguments... args);
```

- The **arguments** template parameter is *zero or more types*
  - These are the **types** of the **corresponding number** of *arguments*, args of the function.
- This example $\to$ the function has **at least one parameter** type $T$ 
  - Can have **any number** of *fixed parameters* alongside **none**.

---

- In the function $\to$ **must unpack** the *parameter* pack to **gain access** to the parameters that are **passed by the caller**
  - Can *determine* how *many items* are there in the **parameter pack** using the **special operator** ==`sizeof...`== 
- The **difference** with the *sizeof* operator $\to$ this is the **item count** and **not the size in bytes**
  - To *unpack* the parameter pack $\to$ must use **the ellipses** on the **right of the name** of the *parameter* pack (example, `args...`)
    - The **compiler** will *expand the parameter pack* at **this point** $\to$ *replacing* the **symbol** with the **contents** of the **parameter pack**.

---

- You are **not aware** at *design time* how many parameters there are or **what types they are**
  - There are *strategies* to **address this**
    - First using **recursion**

```c++
    template<typename T> void print(T t) 
    { 
        cout << t << endl; 
    } 

    template<typename T, typename... Arguments>  
    void print(T first, Arguments ... next) 
    { 
        print(first); 
        print(next...); 
    }
```

- The **variadic templated function** of *print function* can be *called* with *one or more parameters* of **any type** that can **be handled** by the *ostream class*.

```c++
    print(1, 2.0, "hello", bool);
```

- When called
  - The **parameter list** is *split* into two $\to$ the **first parameter** in the first variable and **other three** placed in the **next parameter pack**
    - The *function body* calls the **first version** of *print* thus prints the **first parameter to the console**
- The **next time** $\to$ in the *variadic function* will **expand the parameter pack** in a **call to print**
  - This *calls* oneself **recursively**
    - Within the **call** $\to$ the *first parameter* will be **2.0** and the *rest* are placed in the **parameter pack**
      - This continues till **full parameter pack expansion** so that there are **no more parameters**

---

- Alt way to **unpack parameter pack** is using an **initializer list** 
  - For this case $\to$ the **compiler** creates an **array** for *each parameter*

```c++
    template<typename... Arguments>  
    void print(Arguments ... args) 
    { 
        int arr [sizeof...(args)] = { args... }; 
        for (auto i : arr) cout << i << endl; 
    }
```

- The array **arr** created with **size** of the **parameter pack**
  - Then the **unpack syntax** used with **initializer braces**

- This will work with **any number** of *parameters*
  - All the **parameters** have to be the **same type** of the *array* *arr*.

---

- Trick is using the **comma operator**

```c++
    template<typename... Arguments>  
    void print(Arguments ... args) 
    { 
        int dummy[sizeof...(args)] = { (print(args), 0)... }; 
    }
```

- Creates a **dummy array** called *dummy*
  - This **array** is *not used* other than **within** the **expansion** of the *parameter pack* and the **ellipsis** expands the **parameter pack** using the **expression** that is **between** the *parentheses*.
    - The **expression** uses the **comma operator** 
      - Here $\to$ the **version** of *print* with a **single templated parameter** is *called* with *each item* in the **args parameter pack**.

---

## Overloaded operators

- Function names should **not contain punctuation**
  - Not true entirely $\to$ when **writing an operator**
    - Only use **punctuation** in the **function name**

---

- Operator **used** in some **expression**  *acting* upon *one or more operands*
  - A **unary operator** $\to$ has *one operand*.
  - **binary operator** $\to$ has *two operand* and **an operator returns** the *result of the operation*.
- This **describes** a *function*
  1. return type
  2. name
  3. one or more parameters.
- C++ $\to$ provide **operator** to *indicate* the function is **not used** with the **function call syntax**
  - Rather $\to$ called using the **syntax** associated with **the operator** (often some **unary operator**, first parameter on the **RHS** , and **binary** **LHS**)
    - There are of course **exceptions**

---

- Generally $\to$ provide **the operators** as part of some **custom type**
  - So the **operators** act *upon variables on that type*
- For **some cases** $\to$ can **declare operators** at *global scope*
  - Both are **valid**
- Writing a **custom type** (*classes next section*)
  - Makes sense to **encapsulate** the *code* for **an operator** as *part* of the **custom type**
    - For this section $\to$ **concentrate** on the *other way* to **define an operator** 
      - As some **global function**.

---

- Can *provide* your **own versions** of the *following unary operators*

```c++
  ! & + - * ++ -- ~
```

- And these **binary**

```c++
    != == < <= > >= && ||
    % %= + += - -= * *= / /= & &= | |= ^ ^= << <<= = >> =>>
    -> ->* ,
```

- Write versions of the **function call operator**, array *subscript* [] , **conversion operators**, the *cast operator* () and **new / delete**.
  - You **cannot redefine** the . , .* , ::, ?:, # or ## operators
    - Nor the **named** operators, *sizeof*, *alignof* or *typeid*.

---

- When **defining the** operator $\to$ write a **function** where the *function name* is **operatorx** and **x** is the **operator symbol** (not there is **no space**)
  - example $\to$ define a **struct** that has **two members** *defining* a **cartesian point**
    - May want to **compare** *two points* *for equality*
- The **struct** can be **defined like so**

```c++
    struct point 
    { 
        int x; 
        int y; 
    };
```

- Comparing **two point** *objects* is *easy*
  - They are the **same** if *x and y* of one **object** are *equal* to the **corresponding** values in **other object**.

- If you define the **==** *operator*
  - Then you **should** also *define* the **!= operator** using the **same logic** as **!=** give the **exact opposite** result of the **== operator**
    - This is **how operator** can *be defined*.

```c++
    bool operator==(const point& lhs, const point& rhs) 
    { 
        return (lhs.x == rhs.x) && (lhs.y == rhs.y); 
    } 

    bool operator!=(const point& lhs, const point& rhs) 
    { 
        return !(lhs == rhs); 
    }
```

- The **two parameters** are the *two operands* of the **operator**
  - First is the **operand** on the **left hand side**
  - Second is the **operand** on the **right hand side**
- These are **passed** as *references* so that a **copy is not made**
  - Marked as **const** as the **operator** will *not alter* the **objects**
    - Once defined $\to$ use the **point type** as *such*,

```
    point p1{ 1,1 }; 
    point p2{ 1,1 }; 
    cout << boolalpha; 
    cout << (p1 == p2) << endl; // true 
    cout << (p1 != p2) << endl; // false
```

- Could have defined a **pair** of **functions** called *equals* and *not_equals* and **use these instead**

```c++
    cout << equals(p1,p2) << endl;     // true 
    cout << not_equals(p1,p2) << endl; // false
```

- Defining operators $\to$ makes the **code more readable** as you **use the type** like the **built** in **types**
  - *Operator overloading* is often **referred** to as **syntactic sugar**
    - Syntax that **makes the code** *easier* **read** but this **trivializes** an *important technique*.

- Smart pointers $\to$ are a **technique** that *involves* **class destructors** to *manage resource lifetime*
  - Are **only useful** as you can **call** the **objects** of *such classes* if they are **pointers**

- Can do this **because** the *smart pointer* class **implements** the $->$ and $*$ operators
  - Another example $\to$ **==functors==** or **function objects**
    - Where the **class implements** the **() operator** so *objects* can be **accessed** as if they were **functions**

- When writing a **custom type**
  - Consider if you are **overloading** an *operator* for **your type** actually makes sense
    - If **numeric** $\to$ for example some **complex number / matrix** $\to$ makes sense to **implement arithmetic operators**
      - Would not make sense to implement logical operators as this **does not have a logical aspect**
  - There is **temptation** to *redefine* the **meaning** of *operators* to *cover* the **specific operation** $\to$ makes the **code less readable**.

---

- In general $\to$ a **unary operator** is *implemented as* a **global function** that takes some **single parameter**
  - The *postfix* increment / decrement operators are **exception** to allow for a **different** *implementation* from **prefix operators**.
- *Prefix operators* $\to$ have a **reference** to the **object** as a **parameter** (operator of **increment / decrement**)
  - then return a **reference** to **this changed object**
- *Postfix operator* $\to$ has to **return** the *value* of the **object** *before* the **increment** or **decrement**
  - Therefore the **operator** function has **two parameters**
    1. **REFERENCE** to an *object* to be **changed** 
    2. an **integer** (always 1).
  - returns a **copy** of the **original object**

- A *binary operator* $\to$ has **two parameters** and *returns* an **object** or **reference** to an **object**
  - Example $\to$ **struct** defined **before**
    - Define an **insertion** operator for **ostream** *objects.*

```c++
    struct point 
    { 
        int x; 
        int y; 
    }; 

    ostream& operator<<(ostream& os, const point& pt) 
    { 
        os << "(" << pt.x << "," << pt.y << ")"; 
        return os; 
    }
```

- Can now **insert** a *point* object to the **cout** object and **print** it on the console.

```c++
    point pt{1, 1}; 
    cout << "point object is " << pt << endl;
```

## Function Objects

https://docs.microsoft.com/en-us/cpp/standard-library/function-objects-in-the-stl?view=msvc-160

- A **function object** $\to$ or ***==functor==***
  - Is a **custom type** that *implements* the *function call operator* (**operator()**)
- This means that a **function operator** can be *called* in some way that **looks like a function**
  - Not used classes right now $\to$ explore **function objects types** that are provided by the **standard library** and **how to use**

---

- The `<functional>` header file contains **various types** that can be used as **function objects**
  - Following table lists these:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210704021925628.png)

- These are all **binary function classes**
  - Other than **bit_not, logical_not and negate**
- ==Binary function== objects act on **two values** and **return result**
- ==Unary functions== objects act on a **single value** and **return result** 

---

- Example $\to$ calculate the **modulus** of **two numbers** with **this code**:

```c++
    modulus<int> fn; 
    cout << fn(10, 2) << endl;
```

- Declares a **function object** called `fn` that perform **modulus**
  - The **object** is *used* in the **second line** which calls the **operator()** function on the **object** with **two parameters**
    - Therefore the **following line** is *equivalent* to the **preceding line**:

```c++
    cout << fn.operator()(10, 2) << endl;
```

- Result is the **value 0 ** is printed on the console
  - The **operator()** function performs mod on **the two parameters**

- The `algorithm` header contains **functions** that *work on function objects*
  - Often take **predicates** $\to$ *logical function objects* 
- **transform** $\to$ takes a **function object** that *performs an action*

```c++
  #include <algorithm> 
  #include <functional> 

    vector<int> v1 { 1, 2, 3, 4, 5 }; 
    vector<int> v2(v1.size()); 
	// We calculate the **modulus** of these values with 2 
    fill(v2.begin(), v2.end(), 2); 
    vector<int> result(v1.size()); 

    transform(v1.begin(), v1.end(), v2.begin(), 
        result.begin(), modulus<int>()); 

    for (int i : result) 
    { 
        cout << i << ' '; 
    } 
    cout << endl;
```

- Code performs **five modulus calculations** on the *values* in the **two vectors**
  - Conceptually $\to$ does **this**:

```c++
result = v1 & v2;
```

- Each item in *result* is the **modulus** of the *corresponding* item in `v1` and `v2` 
  - In the **code** $\to$ the *first line* creates a **vector** with *five values*
- We calculate the **modulus** of these values with 2 
  - Therefore the **second line** declares an **empty vector** but with **same capacity** as the **first vector**
- This **second vector** is filled via *calling* the **fill function**
  - The **first parameter** is the *address* of the **first item** in the *vector*
  - The **end function** returns the *address* **after** the **last item** in *vector*
  - The **final item** in the *call* is the **value** that is **placed** in the vector in **every item** starting with the **item** that is **pointed** to by the **first parameter**

---

- The **second vector** contains **five items** and *each one is 2*

---

- Next $\to$ a **vector** is *created* for **the results** 
  - *same size* as the **first array**
    - Finally $\to$ the **calculation** is *performed*

```c++
    transform(v1.begin(), v1.end(),  
       v2.begin(), result.begin(), modulus<int>());
```

- The **first two parameters** give the *iterators* of the *first vector* 
  - From this $\to$ the **number of items** can be **calculated**.
- As all **three** *vectors* are the **same size**
  - Only need the **begin iterator** for `v2` and **result**.

---

- The *last parameter* is the **function object**
  - This is a **temporary object** and exists **during the statement** only with **no name**
    - Syntax here is **explicit call** to the **constructor** of the **class** 
      - It is **templated** therefore you need **to give the template parameter**

- The **transform** will call the **==operator(int, int)==** function on **this function object** for *every item* present in **v1**
  - as the **first parameter** and the *corresponding* item in `v2` as **the second parameter** and it **stores the result** in the **corresponding** *position* in *result*.

---

- As **transform** takes *any binary function object* as the **second parameter**
  - Can pass **instance** of `plus<int>` to **add** a *value* of **2** to every item in `v1` 
    - Alternatively **pass an instance** of `multiplies<int>` to **multiply** *every item* in `v1` by **2**

---

- A **situation** where *function objects* are **useful** $\to$ performing **multiple comparisons** using some *predicate*
  - A **predicate** is a **function object** that *compares* and **returns** a *boolean*
- The `<functional>` header *contains* **several classes** to *allow* you to **compare items**
  - Check how many **items** in the *result* container are **zero** 
    - To do this $\to$ use **count_if** function $\to$ **iterates** over **a container** , apply the **predicate** to *every item* and **count how many times** the *predicate* returns **true**
      - There are **several methods** to **achieve this**
        - First **define a predicate function**

```c++
    bool equals_zero(int a) 
    { 
        return (a == 0); 
    }
```

- A **pointer** to this can be **passed** to the *count_if* function:

```c++
    int zeros = count_if( 
       result.begin(), result.end(), equals_zero);
```

- The **first two parameters** indicate the **range** of *values* to check
  - The **last parameter** is a **pointer** to the **function** that is used as **predicate**
    - If you are checking for **different values** you can **make it more generic**

```c++
    template<typename T, T value> 
    inline bool equals(T a) 
    { 
        return a == value; 
    }
```

- Call as such:

```c++
    int zeros = count_if( 
       result.begin(), result.end(), equals<int, 0>);
```

- Problem $\to$ we are **defining** the *operation* in a place **other** than where its used
  - The **equals** function could be **defined** in *another file*
    - But, with a **predicate** $\to$ its **more readable** to *have the code* that *does* the **checking defined close** to the **code** that requires the **predicate**. 

---

- The `<functional>` header *defines classes* that can be **used** as **function objects**
  - Example $\to$ `equalt_to<int>` which **compares two values**
- The **count_if** function expects a **unary function object** for which it **passes** a *single value*
  - (check out *equals_zero*) previously defined.
- `equal_to<int>` is a **binary function** object comparing **two values**
  - Need to **provide** the **second operand** and to do this **we use the helper function** called *bind2nd*

```c++
    int zeros = count_if( 
       result.begin(), result.end(), bind2nd(equal_to<int>(), 0));
```

- The **bind2nd** will **bind** the *parameter* of **0** to the **function object** create from `equal_to<int>`
  - Using a **function object** like so **brings** the *definition* of the **predicate** *closer* to the **function call** that will *use it*
    - The syntax is messy
      - C++11 provides  a **mechanism** to get the **compiler** to **determine the function objects** that are **required** and *bind parameter* to them
        - These are **lambda expressions**.

# Introducing lambda expressions

- A **lambda expression** used to create an **anonymous function object** at the **location** where the function objects will be used
  - Makes the **code more readable** because you **can see** what will **be executed**
    - On a **first sight** $\to$ a **lambda expression** looks like a **function definition** in *place* as a **function parameter**

```c++
    auto less_than_10 = [](int a) {return a < 10; }; 
    bool b = less_than_10(4);
```

- So we **dont have the complication** of a *function* that uses **a predicate**
  - In **this code** $\to$ we have **assigned** a *variable* to **lambda expression**
- This is **not commonly** how you use it
  - It makes a **description clear** 
    - The *square brackets* at the **beginning** of the **lambda expression** are *called the capture list*.
      - This **expression** does *not capture variables* therefore the **brackets are empty**

- Can use **variables** declared **outside** of the *lambda* and these must be **captured**
  - The *capture list* indices whether **all such variables** are captured via **reference** or **value** ([&] or [=])
    - Can also **name the variable** that **will be captured** (if there are **more than one** $\to$ use **comma separated list**)
      - If captured by value use **just the names**, use & if by **reference**

---

- Can make the **preceding lambda expression** *more generic* by **introducing** some *variable* declared **out** of the *expression* called **limit**

```c++
    int limit = 99; 
    auto less_than = [limit](int a) {return a < limit; };
```

- If you **compare** a *lambda expression* to a **global function**
  - THe **capture list** is a **like** similar to *identifying* the **global variables** that the **global function** *can access*

---

- After the **capture list** $\to$ give the **parameter list** in *parentheses*.
  - If you **compare a lambda** to a function its same overall parameter list, if there are no parameters just **ignore the ()**
- The **body** for *lambda* is given in a **pair of braces**
  - Can *contain anything* that **can be found** in a **function**
    - The **lambda body** can *declare local variables*  / **static variables** , is legal but odd.

```c++
    auto incr = [] { static int i; return ++i; }; 
    incr(); 
    incr(); 
    cout << incr() << endl; // 3
```

- The **return value** of *lambda* is **deduced** from the **item returned**
  - A *lambda expression* does **not have to return a value**, thus can be **void**

```c++
    auto swap = [](int& a, int& b) { int x = a; a = b; b = x; }; 
    int i = 10, j = 20; 
    cout << i << " " << j << endl; 
    swap(i, j); 
    cout << i << " " << j << endl;
```

- THE **POWER** of *lambdas* is that you use when a **function object** / **predicate** is required

```c++
    vector<int> v { 1, 2, 3, 4, 5 }; 
    int less_than_3 = count_if( 
       v.begin(), v.end(),  
       [](int a) { return a < 3; }); 
    cout << "There are " << less_than_3 << " items less than 3" << endl;
```

- Declare a **vector** and **init** it **with some values**
  - The `count_if` function used to **check how many items are less than 3**
    - Give the **range** to scan in the **vector list** which is from **start to end**

- Passes each item in v as `a` to the **lambda expression** which replaces the predicate to determine the count if value.
  - which just tracks number of true values from the lambda.

---

## Using functions in C++

- Example in this **chapter** uses previous techniques to **list all the files** in the *folders / subfolders* in order of **file size** giving a **list of filenames** and the **sizes**
  - Example is equivalent to typing this at the command line:

```c++
dir /b /s /os /a-d folder
```

---

- folder is the **folder** we are *listing*
- /s $\to$ option **recurses**
- /a-d **removes folders** 
- /os - **orders** by **size**
- issue without the /b option we **get information** about each folder but **removing** the **file size** in the **list**
  - We want a **list of filenames** (and *paths*) , with the **size** being **ordered** by **smallest first**.

---

- Start with **creation** of a **new folder** for the chapter

```c++
    #include <iostream> 
    #include <string> 
    using namespace std; 

    int main(int argc, char* argv[]) 
    { 
        if (argc < 2) return 1; 
        return 0; 
    }
```

- This examples uses the **windows functions** of *FindFirstFile* and *FindNextFile*
  - To obtain information about **files** which *meet* some **file specification** 
- These return data in a **WIN32_FIND_DATAA** *STRUCTURE* 
  - This has info on **filename** / **filesize** and **file attributes**
- Function stated also **return information** about folders therefore test **subfolders** and **recurse**
  - ==WIN32_FIND_DATAA== gives **file size** as a **64 bit number** in *two parts*
    1. Upper
    2. Lower 32 bits
- Create **our own** structure to **hold this information**
  - At the **top** after the **C++** include files $\to$ add:

```c++
   using namespace std; 

    #include <windows.h>
    struct file_size {   
        unsigned int high;   
        unsigned int low;
    };
```



