## Working With Expressions

- An expression **is a sequence** of **operators** and **operands** (*variables and literals*)
  - That result into **some value**.

```cpp
int i;
i = 6 * 7;
```

- The  6 * 7 $\to$ is **an expression** 
  - The **assignment** is a **statement**. (from i on the left hand side to the semi colon on the right) is a statement.

---

- All expressions are either **rvalue** / **lvalue**.
  - Used in **error descriptors**.

###### Lvalue

- Refers to a **memory location**.
- Items on the **left hand side** are always **L values**.
- L value can be **on either side**.
- All *variables* are **L values** 

###### Rvalue

- A **Temporary item** that does **not exist** longer than **some expression** that uses its.
- Has **some value**.
- Can **not** have some **value assigned to it**.

- Can **only exist** on the **right hand side** of *an assignment*.
- Any *literals* are **rvalues**.

```cpp
int i;
i = 6 * 7; // i = l value, 6 * 7 is a rvalue of 42.

6 * 7 = i; // will not compile

```

---

- Expression $\to$ statement when you add **semicolon**.

```c++
42; // r value of 42 , it has no effect as its temporary. c++ compiler just optimises it away.
std::sqrt(2); // calls the standard liibrary function to calculate the square root of 2, result is an r value and the value is not used therefore the compiler will optimise it away.
//both statements
```

## Using the Comma Operator

- Sequence of legal expressions separated by a comma > **single statement**
  - Legal code in C++

```cpp
int a = 9;
int b = 4;
int c;
c = a + 8, b + 1;
```

- Compiles and runs $\to$ variable $c$ assigned a value of 17.
- Comma **separates** the **RHS** of the *assignment* into **two expression**.
  - Executed **left** to **right** so *the value* of **group of expressions** is *right most one*
- There are **some cases**
  - For example $\to$ in **the init** or **loop** *expression* $a$ *for loop*
    - Where **you find the comma operator** useful $\to$ produces **hard to read code** therefore dont.

## Using Types and Variables

- C++ $\to$ you **strongly typed language**.
  - Must **declare the type** of the *variables* that you use.
- The compiler must **know** *how much memory* to *allocate*
  - Can **determine** this by **type of variable**.
- Compiler needs to know how **to initialise variable** 
  - If not **explicitly initialized** 

---

- Auto *relaxes* the **idea** of **strong typing**.

---

- Variables defined anywhere , **must be declared **
- Scope matters, location of its definition.
- Best to **declare the variable** as *close as possible* to **where using it**.
  - Within **its restrictive scope** also.
    - Prevent **name clashes** $\to$ where you add **additional information** to *disambiguate* **two or more variables**.

---

- Variable names are **case sensitive**.

---

- Declare a **variable** in a **statement.**
  - Ending **with a semicolon**.
- Syntax of **declaring variable** is that **you specify the type**
  - Then the **name** , **optionally** $\to$ *initialisation* of the **variable**.

---

- *Built in types* must be **initialised** 

```c
int i;
i++; // C4700 uninitialized local variable 'i' used
std::cout << i;
```

- There are **3 ways** to *initialize* variables

  1. Can **assign a value** 

  2. call the **type constructor** (*constructors for classes* are **defined** in *chapter 4*, classes) 

  3. Init a variable using **function syntax**

```c
int i =1;
int j = int(2);
int k(3);
```

- These are **all legal** in C++
  - *Stylistically* the **first** is *better* as its **obvious**.
    1. Variable is **integer**
    2. Called $i$ 
    3. Assigned **value** of $1$.
- Third is **most confusing**
  - Seems to be like **function declaration** but actually *declaring a variable*.

---

- Classes $\to$ create **custom types**
  - *Custom type* can be **defined** with some *default value*
    - May decided **against initialization** of some *custom type* before **using it** $\to$ results **in poor performance**
- Compiler will **initialize** the *variable* with **the default value** and **subsequently** your *code* **will assign a value**
  - Results **in an assignment** being **performed twice**

---

## Using constant and literals

- Each **type** has a **literal representation**
  - Integer has **numeric represented** with **no decimal** point
  - If **signed integer** $\to$ plus or minus.
  - Real number can **have literal value** with a **decimal point** , may use **scientific** or **engineering** format *including an exponent*
- C++ $\to$ has various rules, literals shown here.

```cpp
int pos = +1;
int neg = -1;
double micro = 1e-6;
double unit = 1.;
std::string name = "Richard";
```

- Note $\to$ the **unit variable** 
  - Compiler **knows** that the *literal* is a **real number** as the value has **decimal point**
- Integers $\to$ provide **hexadecimal literal** in **your code** by *prefixing* the *number* $0\text{x}$, $0\text{x}100$ is **256** in **decimal**.
  - Default **output stream** prints in **base 10**.
    - Use a **manipulator** to the output stream **to tell** it to use a **different base**.

##### Manipulators

- std::dec
- std::hex
- std::oct
- std::showbase

---

- cpp define **some literals**
  1. bool $\to$ **logic type** of **true  / false constants**
  2. nullptr $\to$ **again** / **zero** $\to$ used as **an invalid value** for *any pointer type*

---

## Defining constants

- Provide **constants** *values* that **are used throughout** the *code*
  - Example $\to$ decide to **declare a constant** for $\pi$.
- Not allowed to **change this value**
  - Used to provide **compiler errors** suitably.

```cpp
const double pi = 3.1415;
double radius = 5.0;
double circumference = 2 * pi * radius;
```

- The pi is declared as **constant**.
  - Therefore **cannot change**, if **you subsequently decide** to change **the constant** the *compiler* issue **an error**:

```cpp
// add more precision, generates more c3892
pi += 0.00009265359;
```

- Once **you have declared constant**, you *can be assured* **the compiler** will *make* sure **it remains** *so*
  - You **assign** a **constant** with **an expression** as *follows:*

```cpp
#include <cmath>

const double sqrtOf2 = std::sqrt(2);
```

- A **global constant** called *sqrtOf2* $\to$ *declared* and **assigned** with some value std::sqrt function
  - As this **constant** is *outside a function* $\to$ its **global** to the **file** and can be **used anywhere** in **the file** 
- The **problem** is that the **pre-processor** does a **simple replacement**
  - With constants **declared** *declared* with **const** $\to$ the **C++ compiler** will *perform type checking* to **ensure** that the **constant** is being **used appropriately**. 

- Can **use const** to *declare* some constant that is **used** as **constant expression**
  - Declare **array** using **square bracket syntax**.

```cpp
int values[5];
```

- Declares **array** of **five integers** on **the stack**
  - Accessed through the **values array** 
- The $5$ is **constant expression**
- When **declaring** some array on **the stack**
  - must provide **compiler** with **constant expression** so it knows **how much memory** to *allocate* therefore the **size** of the **array** must **be known at compile time**.
- Can *allocate array* with **a size** known **only at runtime**
  - But **this requires** *dynamic memory allocation* $\to$ **explained** 
    - Working **with memory, arrays, and pointers**.
- In C++ $\to$ **declare** a **constant** to *do following*

```c
const int size = 5;
int values[size];
```

- use **size variable** to check accesses **are not ** beyond that **age**.
  - Since **the** *size* variable $\to$ declared **in one just one place** $\to$ change size **of array at later stage** in one place.
- Const **keyword** can *also* be **used** on **pointers and references** / objects / parameters functions
  - USE CONST IN PARAMETERS TO HELP COMPILER ENSURE THE POINTERS, REFERENCES AND OBJECTS ARE USED APPROPRIATELY.

## Using constant expression

- C++11 > **constexpr**
  - Applied to **some expression** $\to$ indicates **the expression** should be *evaluated* at **compile type** rather than **runtime**

```cpp
constexpr double pi = 3.1415;
constexpr double twopi = 2 *pi;
```

- This **is similar** to *initializing* a **constant** declared with **const keyword**
  - However **constexpr** can be **applied** to **function** that **return** a **value**.
    - Can be **evaluated** at *compile time* and so **this allows compiler** to **optimize code**.

```cpp
constexpr int triang(int i){
    return (i == 0) ? 0 : triang(i - 1) + i;
}
```

- In **this example** $\to$  function $\text{triang}$ calculates **numbers recursively**.
  - Code **uses** the **conditional operator** $\to$ in the *parentheses* $\to$ **function parameter** is *tested* to **see if zero**
- If function returns  0 $\to$ this ends the recursion
  - return function to **original caller**.
- If **parameter** is **not zero** $\to$ then **return value** is **sum** of the *parameter* and **return and** the *value* of $triang$ with **parameter is decremented**.

---

- Called **with a literal in your code** $\to$ can be *evaluated* at **compile time**
  - The *constexpr* is **indication** to the **compiler** to **check usage** of *function* to see if it can **determine** the *parameter* at **compile time**
- If so $\to$ the **compiler** can **evaluate** the **return value** and **produce code more efficiently** than by **calling function at runtime**
  - If compiler can **not determine** the *parameter* at **compile-time** $\to$ function called **normally** 
- Function **with constexpr** must have **one expression**
  - Hence the **use of conditional operator** ?: in the **triang function**

## Using enumerations

- Provide constants **using** some **enum variable**
  - In effect $\to$ an **enum** is a **group** of *named constants* $\to$ means you **can use an enum** as **parameter to a function**.
  - For example:

```cpp
enum suits {clubs, diamonds, hearts, spades}
```

- Defines **enumeration** that is *called suits*.
  - *Named values* for **suits** in **a deck of cards** 
- An **enumeration** $\to$ an **integer type** and **by default** the compiler assumes **integer**
  - can **specify** the **type yourself** in *declaration*
    - Waste space to store in **integer** as **char** is just **1 byte next to 4**

```c++
enum suits: char {clubs, diamonds, hearts, spades}
```

- Using an **enumerated value**
  - Use the **actual name** $\to$ usual to **scope it** with **the name of enumeration** to enable **increasing readability**

```c++
enum suits: char {clubs, diamonds, hearts, spades}

suits card1 = diamonds;
suits card2 = suits::diamonds;
```

- **Both forms** are *allowed* 
  - The **latter** makes **more explicit** that the **value** is *taken* off some **enumeration**.
    - Forces **developers** to *specify the scope* $\to$ apply keyword **class**
- With this $\to$ the line declaring card **2 compiles** but not the preceding one

```c++
enum class suits: char {clubs, diamonds, hearts,spades};
suits card = suits::diamonds;
char c= card + 10; // errors c2784 and C2676
```

- The **enum** is based **on char** when **you define** the *suits variable* as **being scoped** with *class*
  - Second line **will not compile**.
- If **the enumeration** is defined as **not being scoped** *without class*.
  - There is the **inbuilt conversion** of the **enumerated value** and *char*. 

---

- Default $\to$ **compiler** will **give the first enumerator** a *value* of $0$ and **then incremented** the *value* for the **subsequent enumerators**
  - Therefore **suits::diamonds** has value of 1 as the **second value**.

```c++
enum ports {ftp = 21, ssh, telnet, smtp = 25, http = 80};
```

- This case $\to$ **ports::ftp** has 21, ssh, telnet both have 22.
  - ports::smtp = 25, ports::http = 80.

> The **purpose** of *enumerations* is to *provide* **named symbols** within **your code** and *their values* are **unimportant**.
>
> No one cares of the values of suits::hearts just to ensure they are different to clubs really.

- **Enumerations** are *useful* in a **switch statement** 
  - because **the name value** makes it **clearer** than **using just an integer**
    - Can **also use** an **enumeration** as a **parameter** to a **function** and **hence restrict** the values **passed** via **that parameter**:

```cpp
void stack(suits card){
    // we know that card is only one of four values
}
```

## Declaring pointers

- Access memory **using typed pointer**
  - The **type** indicates the **type** of *data* that is **held in memory**
  - Pointer **is an 4 byte integer pointer** $\to$ point to **4 bytes** that are used as an integer.
    - if the integer **pointer is incremented** $\to$ it **will point** to the **next four bytes** $\to$ can used **as an integer**
- GO TO C POINTER NOTES, SAME THING.

---

## Using Namespace

- Namespaces **give** you *one mechanism* to **modularize code**
  - A *namespace* allows you to **label your types**, **functions** and **variables** with some *unique name*.
    - Use the **scope resolution operator** $\to$ give some **fully qualified name**. 
- Advantage $\to$ know **exactly** the *item to be called* 
- Disadvantage $\to$ using **qualified name** is switching **off** the **C++** argument **dependent lookup mechanism** for **overloaded functions**#
  - Compiler chooses **function** that has the **best fit** *according* to the **arguments** that are **passed to the function**.

---

- Define a namespace
  - Just *decorate* $\to$ **types / functions** and **global variables** with the *namespace* *keyword*
    - Along with the **name** you **give to it**.
- *Two functions* are defined **in this namespace**:

```cpp
    namespace utilities 
    { 
        bool poll_data() 
        { 
            // code that returns a bool 
        } 
        int get_data() 
        { 
            // code that returns an integer 
        } 
    }
```

- NOTE THERE ARE NO SEMICOLONS AFTER CLOSING BRACKET

- Use the **symbols**
  - Must **qualify** the name with **some namespace**

```cpp
    if (utilities::poll_data()) 
    { 
        int i = utilities::get_data(); 
        // use i here... 
    }
```

- Namespace **declaration** may just **declare functions** $\to$ actual functions **would be defined elsewhere**
  - Must use some **qualified name** 

```cpp
    namespace utilities 
    { 
        // declare the functions 
        bool poll_data(); 
        int get_data(); 
    } 

    //define the functions 
    bool utilities::poll_data() 
    { 
        // code that returns a bool 
    } 

    int utilities::get_data() 
    { 
       // code that returns an integer 
    }

```

- A use of *namespaces* $\to$ to **version your code**
  - The *first version* of **your code** $\to$ may have some **side effect** that is **not in your functional specification** 
    - Some callers use and **depend** on it.
- Update $\to$ **fix bug** $\to$ may *decide* to allow the callers **the option** to use **the old version** so the code **does not break**.
  - Achieved with a **namespace**.

```cpp
namespace utilities 
{
    bool poll_data();
    int get_data();
    
    namespace V2
    {
        bool poll_data();
        int get_data();
        int new_feature();
    }
}
```

- Can specify versions now
  - $\text{utilities::V2::poll_data}$ for **new version**, and just **utilities::poll_data** to use **older version**

- Item **within some namespace** calling  an **item in the same namespace** $\to$ does *not require qualified name*
  - If **new_feature** calls **get_data** $\to$ utilities::V2::get_data that is **called**.
    - Note that $\to$ to **declare** some **nested namespace**  must do **manual nesting**
      - Cannot just **declare** some *namespace* called **utilities::V2**

- C++ provides facility called **inline namespace**
  - Enables you to **define** a **nested namespace**
    - Allows the **compiler** to *treat the items* as being **in same parent namespace** when it **performs argument dependent lookup**

``` cpp
    namespace utilities 
    { 
        inline namespace V1 
        { 
            bool poll_data(); 
            int get_data(); 
        } 

        namespace V2 
        { 
            bool poll_data(); 
            int get_data(); 
            int new_feature(); 
        } 
    }
```

- Can use utiltiies:get_data or utilities::V1::get_data

---

- **Fully qualified names** make it **difficult to read code**
  - Particularly if using **a single namespace**.
- Have many options:
  1. Place **using statement** to *indicate* that **symbols** declared in **specified namespace** can be used with **no fully qualified name**.

```cpp
using namespace utilities;
int i = get_data();
int j = V2::get_data();
```

- Namespace can **contain many items**
  - May **decide** that you **only want to relax** the use of **fully** *qualified names* with **just a few of them**
  - Use the **using name**

```cpp
using std::cout;
using std::endl;

cout << "Hey" << endl;
```

- Do **not have** to *declare* some *namespace* in **one place**
  - Can be **over several files**
    - Following can be **different file** to **previous declaration** of *utilities*

```cpp
namespace utilities
{
    namespace V2
    {
        void print_data();
    }
}
```



- print_data $\to$ **still part** of *utilities::V2* **namespace**.

---

- Can use **#include** in *namespace* $\to$ where the items **declared** in the **header file** are now **part of the namespace**
  - The **standard library** header files have **c prefix** (*cmath*, *cstdlib*, *ctime*) 
    - Gives access to **the C runtime function** by *including* the **appropriate C header** in the *std namespace*.

---

- Massive **advantage** of a **namespace**
  - **Defining** items with **names** that **may be common** but are **hidden from other code** that *does not know* the **namespace of**
- The **namespace** means the **items** are *still available* to **your code** via **fully qualified name**.
  - Only works if using **unique namespace name**
    - The **likelihood** is that the **longer namespace name** $\to$ the **more unique** it is **likely to be**

- Java devs $\to$ often **name** classes using **URI**
  - Could decide **same thing**

```cpp
    namespace com_packtpub_richard_grimes 
    { 
        int get_data(); 
    }
```

- Makes qualified name very long
  - Get around **using alias**

```cpp
namespace packtRG = com_packtpub_richard_grimes;
int i = packtRG::get_data();
```

- Can define **anonymous namespace**
  - Namespaces allow you **to prevent name clashes** between code in **several files**
- Defining a name in **one file** $\to$ you could **define unique namespace name**
  - This can get annoying for **several files**
    - A **no name namespace** has an **internal linkage** $\to$ the **items** can **only be used** within the **current translation unit**
      - This is the **current file** and **no other files**
- Code **not within namespace** $\to$ member of the **global namespace**
  - This is called **without a name**
    - indicate the item is global using the **scope resolution operator** with **no namespace name**.

```cpp
    int version = 42; 

    void print_version() 
    { 
        std::cout << "Version = " << ::version << std::endl; 
    }
```

## C++ Variable Scoping

- Compiler $\to$ **compiles** each *source file* $\to$ as **individual items** called **translation units**
  - Compiler **determines** the **objects** and **variables** that you **declare** and the **types** and **functions** you **define**
    - Declared $\to$ use **any** within **subsequent code** in the **scope of declaration**.
- Declare globally within a **header file** $\to$ used by **all source files** within **some project**
  - If you **dont** use some **namespace** $\to$ its **often wise** to use **global variables** to *name* them as **part of global namespace**

```cpp
    // in version.h 
    extern int version; 

    // in version.cpp 
    #include "version.h"  
    version = 17; 

    // print.cpp 
    #include "version.h" 
    void print_version() 
    { 
        std::cout << "Version = " << ::version << std::endl; 
    }
```

- Has c++ for **two source files**
  - version.cpp and print.cpp
- A **header file**
  - version.h $\to$ included by **both source files** 
    - Declares global variable of **version** $\to$ used by **both source files**
    - Declares but **not defines** $\to$ done in **version.cpp** 
      - Compiler here **allocates memory** for that variable.
- **EXTERN** keyword $\to$ used on **declaration** in the **header indicates** to compiler that the version has **external linkage**
  -  The **name** is **visible** in files **other** than where the **variable is defined**
- *version* variable used in **print.cpp** source file
  - In this file $\to$ **scope resolution operator ::** used with **no namespace name** and therefore **indicates** the **variable** *version* is within **global namespace**

---

- Declare items only used by **single translation unit**
  - Declaring them **within source file** right **before** they are used $\to$ often at **top of file**
    - Produces level of **modularity** and enables you **to hide implementation** details from **code** in *other source files*
      - Examples:

```cpp
    // in print.h 
    void usage(); 

    // print.cpp 
    #include "version.h" 
    std::string app_name = "My Utility"; 
    void print_version() 
    { 
       std::cout << "Version = " << ::version << std::endl; 
    } 

    void usage() 
    { 
       std::cout << app_name << " "; 
       print_version(); 
    }
```

- print.h header **contains** the **interface** for the code in the **print.cpp** 
  - Only the functions **declared in the header** are *callable* to **other source files**
- The **caller** does *not need to know* of the **implementation** of *usage* function
  - Can **see** its **implemented** using **call to function** $\to$ print_version $\to$ only available to **code** in print.cpp
    - Variable app_name is **declared ** at **file scope** therefore only **accessible** to code **in print.cpp**.

---

- If **another source file** declares a **variable** at **file scope** called app_name
  - also a std::string $\to$ the **file will compile** however the **linker** will **complain** when it attempts to **link object files**
    - Reason $\to$ the **linker** will *see same variable* defined **in two places** and **undefined** on **which to use**.

---

- A **function** also **defines scope**
  - Variables **within** are only *accessed* **via that name**
  - *Parameters* of the **function** are included as **variables** within the **function** 
  - Therefore use **seperate names** when *declaring inside* 
  - If **non const parameter** $\to$ can **alter** the **value of the parameter in the function**

---

- Declare **variables** *anywhere* in function as long as **you declare** *before using*
  - Curly braces define **code blocks** $\to$ defines **local scope**
    - Can define some variable in a code block and **still use outside of that scope**

---

- Mention aspect of **C++ storage class**
  - A **variable** declared in a **function** means that the **compiler** will **allocate memory** for the *variable* on the **stack frame** CREATED FOR **this function**
    - When the **function ends** $\to$ stack frame is **torn down** and **memory recycled** 
      - After returning $\to$ the **values** in **any local variables** are **lost**
- When the **function** is **called again** $\to$ the **variable** is **created anew** and **initialized again**.

---

- C++ has **static keyword** to alter this
  - **static** means the **variable** is *allocated when the program starts* like **variables in global scope**
- Applying **static** to a **variable** declared in **function** means the **variable** has **internal linkage**
  - The **compiler restricts** access to **that variable to that function**.

```cpp
    int inc(int i) 
    { 
        static int value; 
        value += i; 
        return value; 
    } 

    int main() 
    { 
        std::cout << inc(10) << std::endl; 
        std::cout << inc(5) << std::endl; 
    }
```

- Default $\to$ compiler **initializes** a *static variable* to $0$ 
  - Can **give some value yourself** $\to$ used when variable **first allocated**
- When program **starts** $\to$ the *value variable* is **initialized** to $0$ **before** the **main function** is actually **called**
  - The *first time* the **inc function** is *called* $\to$ the **value** *variable is incremented to 10*
    - Which is *returned* by the **function** and **printed to console**
- The variable stays same and is not wiped off stack frame.

---

