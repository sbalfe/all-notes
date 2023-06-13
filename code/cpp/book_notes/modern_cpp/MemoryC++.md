# Memory in C++

## Pointer Syntax

- & $\to$ return **address of the object**.
- This object $\to$ assigned a **typed pointer variable** or **void*** 
  - void* treated often as **storage** for said **memory address** as you can **not access** or **perform pointer arithmetic**.

```c++
int i = 42; // compiler and linker determine this variable where its allocated
int *pi = &i;
```

- Variable stored on the **stack frame**.
- **Runtime** $\to$ stack is created , **chunk of memory**
  - Space **reserved** in the **stack memory** for the variable $i$. 
  - Places $42$ within this **section**.

- The **address** of memory allocated for **variable i** placed within **variable pi**
  - The **memory usage** of the **previous code** shown below.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210607235629936.png)

---

- The pointer holds a **value of 0x007ef8c** (notice that the lowest byte is stored in the **lowest byte in memory**; this is for an **x86 machine**). The memory location **0x007ef8c** has a value of **0x0000002a**, that is, a value of 42, the value of the variable i.
-  Since **pi is also a variable**, it also **occupies space** in **memory**, and in this **case the compiler** has put the **pointer *lower* in memory** than the **data** it points to and, in this case, the **two variables** are **not contiguous.**

- Do not **assume** where the variables are allocated, neither their **location** in **relation to other variables**.

---

- This code assumes a **32-bit operating system**, and so the **pointer pi occupies 32 bits** and contains a 32-bit address. 
- If the operating system is **64 bits** then the pointer will be 64 bits wide (but the integer may still be 32 bits). 
- In this book, we will use **32-bit pointers** for the simple convenience that 32-bit addresses take less typing than 64-bit addresses.

- The **typed pointer is declared with a *** symbol and we will refer to this as an int* pointer because the pointer points to memory that holds an int. 
- When declaring a pointer, the convention is to put the * next to the variable name rather than next to the type. This syntax emphasizes that the *type pointed* to is an int. 
- However, it is important to use this syntax if you declare more than one variable in a single statement:

- Dereference operator **does more than give read access** to the data at the memory location.
  - You can use **deference** to **write memory**

```c++
    int i = 42; 
    cout << i << endl; 
    int *pi { &i }; 
    *pi = 99; 
    cout << i << endl;
```

- Using braced syntax for init.

https://stackoverflow.com/questions/18222926/why-is-list-initialization-using-curly-braces-better-than-the-alternatives

## Null pointers

- A pointer could point to **anywhere in the memory** installed in your computer, and assignment through a **dereferenced pointer** means that you **could potentially write** over **sensitive memory** used by your **operating system**, or (through direct memory access) write to memory used by hardware on your machine.
- However, **operating systems** will usually give an **executable a specific memory range** that it can **access**, and attempts to **access memory out** of this **range** will cause an **operating system** memory access violation.

---

- Always **obtain pointer values** using the $\&$ operator or **from a call** to an **operating system function**
  - Do **not give a pointer** an **absolute address** $\to$ only *exception* to this is **C++ constant** for an **invalid memory address** *nullptr*:

```c++
int *pi = nullptr; // init to nullptr 
// code
int i = 42;
pi = &i;
// code
if (nullptr != pi) cout << *pi << endl;
```

- This code initializes the pointer pi to nullptr. 
- Later in the code, the pointer is initialized to the address of an integer variable.
- Still later in the code, the pointer is used, but rather than calling it immediately, the pointer is first checked to ensure that it has been initialized to a non-null value. 
- The compiler will check to see if you are about to use a variable that has not been initialized, but if you are writing library code, the compiler will not know whether the callers of your code will use pointers correctly.

---

> The type of constant nullptr is not an integer, it is **std::nullptr_t**. All pointer types can be implicitly converted to this type, so nullptr can be used to initialize variables of all pointer types.

## Types of Memory

- One of **4 types**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210608011615328.png)

- Global / Static $\to$ compiler **ensures** the variable stays in memory for the **entire lifetime of the application**

---

- The **string literal** $\to$ same as **global / static** , however stored in a **different** part of some **executable**
  - For some **windows executable** $\to$ string literals are stored in the **.rdata** PE/COFF

https://stackoverflow.com/questions/2589949/string-literals-where-do-they-go

---

- Visual C++ allows you the option of a **string pooling**

```c++
    // two pointers are initialized with the address of the string literal hello
	char *p1 { "hello" }; 
    char *p2 { "hello" }; 
	// in the following two lines, the address of each pointer on the console
    cout << hex; 
	// << operator for  char * treats the variable as pointer to string
    cout << reinterpret_cast<int>(p1) << endl; // call reinterpret_cast operator to convert pointer to integer 
    cout << reinterpret_cast<int>(p2) << endl;
```

https://stackoverflow.com/questions/573294/when-to-use-reinterpret-cast

---

- Compiling code in command line of visual C++ compiler 
  - See **two different addresses ** printed, these addresses are in the **.rdata** section are **read only**
- Compiling /GF switch $\to$ enable **string pooling** $\to$ compiler see that the **two string literals** are the **same** and only **store one copy** in the **.rdata** section
  - So **the result of this code** will be that **a single address** will be printed **on the console twice**

https://docs.microsoft.com/en-us/cpp/build/reference/gf-eliminate-duplicate-strings?view=msvc-160

---

- p1 / p2 are **automatic variables** $\to$ they are **created on the stack** created for the **current function**
  - When a **function** is called $\to$ a **chunk of memory** is allocated for **the function** and this has space for **parameters** passed to the function and the **return address** of the **code** that called the function
  - Space for **automatic variables** declared in **the function**
  - Function finishing **destroys the stack frame**



>*The* **calling convention** *of the function determines whether the calling function or the called function has the responsibility to do this. In Visual C++, the default is the* $\text{__}$cdecl *calling convention, which means the calling function cleans up the stack. The* __stdcall *calling convention is used by Windows operating system functions and the stack clean up is carried out by the called function. More details will be given in the next chapter.*

- Automatic variables only last as long as the function and the address of such variables only make any sense within the function. 
- Later in this chapter, you will see how to create arrays of data. **Arrays** allocated as **automatic** **variables** are **allocated** on the **stack** to a fixed size **determined at compile time.**
- It is possible with large arrays that you could exceed the size of the stack, particularly with functions that are called recursively.
- On Windows, the default stack size is 1 MB, and on x86 Linux, it is 2 MB. Visual C++ allows you to specify a bigger stack with the /F compiler switch (or the /STACK linker switch). The gcc compiler allows you to change the default stack size with the --stack switch.

---

- **Dynamic memory** $\to$ created on **free store** / **heap** 
  - Allocation at **runtime**
  - Dependent on **C++ implementation** , regard as **same life** as **the application**
- Must be **returned to free store after use**.

## Pointer arithmetic

- A pointer points to memory, and the type of the pointer determines the type of the data that can be accessed through the pointer. So, an int* pointer will point to an integer in memory, and you dereference the pointer (*) to get the integer. If the pointer allows it (it is not marked as const), you can change its value through pointer arithmetic. 
- For example, you can increment or decrement a pointer. What happens to the value of the memory address depends on the type of the pointer. Since a typed pointer points to a type, any pointer arithmetic will change the pointer in units of the *size* of that type.

- If you increment an int* pointer, it will point to the *next* integer in memory and the change in the memory address depends on the size of the integer. 
- This is equivalent to array indexing, where an expression such as v[1] means you should start at the memory location of the first item in v and then move one item further in memory and return the item there:

```c++
    int v[] { 1, 2, 3, 4, 5 };
    int *pv = v;
    *pv = 11;
    v[1] = 12;
    pv[2] = 13;
    *(pv + 3) = 14;
```

- The first line allocates an array of five integers on the stack and initializes the values to the numbers 1 to 5. In this example, because an initialization list is used, the compiler will create space for the required number of items, hence the size of the array is not given.
- If you give the size of the array between the brackets, then the initialization list must not have more items than the array size. If the list has fewer items, then the rest of the items in the array are initialized to the default value (usually zero).
- The next line in this code obtains a pointer to the first item in the array. This line is significant: an array name is treated as a pointer to the first item in the array. The following lines alter array items in various ways. 
- The first of these (*pv) changes the first item in the array by dereferencing the pointer and assigning it a value. The second (v[1]) uses array indexing to assign a value to the second item in the array. *
- *The third (pv[2]) uses indexing, but this time with a pointer, and assigns a value to the third value in the array. *
- *And the final example (*(pv + 3)) uses pointer arithmetic to determine the address of the fourth item in the array (remember the first item has an index of 0) and then dereferences the pointer to assign the item a value. 
- After these, the array contains the values { 11, 12, 13, 14, 5 } and the memory layout is illustrated here:

![img](https://learning.oreilly.com/library/view/modern-c-efficient/9781789951738/assets/1ca5a666-72a0-4412-895f-6e331bb5042d.png)

If you have a memory buffer containing values (in this example, allocated via an array) and you want to multiply each value by 3, you can do this using pointer arithmetic:

```c++
    int v[] { 1, 2, 3, 4, 5 }; 
    int *pv = v; 
    for (int i = 0; i < 5; ++i) 
    { 
        *pv++ *= 3; 
    }
```

- The loop statement is complicated, and you will need to refer back to the operator precedence given in [Chapter 1](https://learning.oreilly.com/library/view/modern-c-efficient/9781789951738/c1d6db13-db92-4b05-96b2-1f36dbce3ced.xhtml), *Understanding Language Features*. 
- The postfix increment operator has the highest precedence, the next highest precedence is the dereference operator (*), and finally, the *= operator has the lowest of the three operators, so the operators are run in this order: ++, *, *=. 
- **The postfix operator returns the value *before* the increment, so although the pointer is incremented to the next item in memory, the expression uses the address before the increment. This address is then dereferenced which is assigned by the assignment operator that replaces the item with the value multiplied by 3.** 
- This illustrates an important difference between pointers and array names; you can increment a pointer but you cannot increment an array:

```
    pv += 1; // can do this 
    v += 1; // error
```

You can, of course use indexing (with []) on both array names and pointers.

## Using arrays

- As the name suggests, a C++ built-in array is zero or more items of data of the same type. In C++, square brackets are used to declare arrays and to access array elements:

```c++
    int squares[4]; 
    for (int i = 0; i < 4; ++i)  
    { 
        squares[i] = i * i; 
    }
```

- The **squares variable** is an array of integers. The first line allocates enough memory for *four* integers and then the for loop initializes the memory with the first four squares. 
- The **memory allocated** by the compiler from the stack is **contiguous** and the items in the array are sequential, so the memory location of squares[3] is **sizeof(int) following on from squares[2].**
- Since the array is created on the stack, the size of the array is an instruction to the compiler; this is **not dynamic allocation**, so the size has to be a constant.

---

- There is a potential problem here: the **size of the array** is **mentioned twice**, once in the declaration and then again in the for loop. If you use two different values, then you may initialize too few items, or you could **potentially access memory outside the array**. 
- The **ranged for syntax** allows you to get access to each item in the array; the **compiler can determine the size** of the array and will use this in the ranged for loop. 
- In the following code, there is a deliberate mistake that shows an issue with array sizes:

```c++
    int squares[5]; 
    for (int i = 0; i < 4; ++i)  // leaves random unit variable for last item, also cannot have 3 items in squares as this will write out of bounds in this case.
    { 
        squares[i] = i * i; 
    } 
    for(int i : squares) 
    { 
        cout << i << endl; 
    }
```

---

- Methods way to **mitigate these issues**
  - Declaring a **constant** for **the size** of the **array** and use that whenever the **code** requires to **know the array size**

```c++
    constexpr int sq_size = 4; 
    int squares[sq_size]; 
    for (int i = 0; i < sq_size; ++i) 
    { 
        squares[i] = i * i; 
    }
```

- The **array declaration** must have **constant for its size**
  - This is **managed** using the **sq_size** *constant variable*.
- May wish to **calculate** the **size** of some **already allocated array**
  - The *sizeof* operator to an array returns **size in bytes** of **entire array** 
    - Therefore determine the size of the array by **dividing** by the **size of single item**

```c++
    int squares[4]; 
    for (int i = 0; i < sizeof(squares)/sizeof(squares[0]); ++i) 
    { 
        squares[i] = i * i; 
    }
```

- This **is safer** however **longer** 
  - C **Runtime** library has a **macro** called **_countof** to **perform this calculation**

## Function Parameters.

- There is **automatic conversion** of some **array** to specific **pointer type** 
  - This occurs when **passing** an **array to a function** or **return** it from a **function**.

- This **decay to a dumb pointer** means that **other code** can make no assumption about an array size.
-  A pointer could point to **memory allocated** on the **stack** where the memory lifetime is **determined by the function**, or a global variable where the memory lifetime is that of the program, or it could be to memory that is dynamically allocated and the memory is determined by the programmer. 
- There is nothing in a pointer declaration that indicates the **type of memory** or who is responsible for the deallocation of the memory. 
- Nor is there any information in a dumb pointer of **how much memory the pointer points to**. When you write code using pointers, you have to be disciplined about how you use them.

---

- A function can **have an array parameter**.
  - This means a **lot less** than it **appears to indicate**.

```c++
    // there are four tires on each car 
    bool safe_car(double tire_pressures[4]);
```

- This function will check that each member of the array has a value between the minimum and maximum values allowed. 
- There are four tires in use at any one time on a car, so the function *should* be called with an array of four values. 
- The problem is that although it appears that the compiler *should* check that the array passed to the function is the appropriate size, it doesn't. You can call this function like this:

```c++
    double car[4] = get_car_tire_pressures(); 
    if (!safe_car(car)) cout << "take off the road!" << endl; 
    double truck[8] = get_truck_tire_pressures(); 
    if (!safe_car(truck)) cout << "take off the road!" << endl;
```

- sanity checks are not applied here , but not for the case **of array parameters such as this.**

https://stackoverflow.com/questions/4055733/what-is-a-sanity-test-check

- The array is **passed as a pointer**
  - The parameter appears to be some **built in array** $\to$ you **cannot use facilities** you are used to **using** with **arrays** like **ranged for**
- If the **safe_car** function calls sizeof(tire_pressures)
  - It get the **size of double pointer** and **not 16** (the **size in byte of a four int array**)

---

- This **decay** to a **pointer** feature of **array parameters** means functions only ever know **size of an array parameter** if you **explicitly** tell **it the size**.
  - Use **empty pair** of **square brackets** to indicate the **item should be passed as an array**, but its the **same as a pointer**

```c++
    bool safe_car(double tire_pressures[], int size);
```

- Here **the function** has a parameter to **tell size**
  - This function is the **same** but rather declares as a **pointer**

```c++
   bool safe_car(double *tire_pressures, int size);
```

- When passing an **array** to a **function**
  - The **first dimension** of the **array** is treated as **a pointer** 
    - For arrays have been **single dimensions so far**.

## Multidimensional arrays

- Add more **square brackets**

```c++
    int two[2]; 
    int four_by_three[4][3]; // 4 rows and 3 columns
```

- **Init** of multi dimensions involves **braces pair**

```c++
    int four_by_three[4][3] { 11,12,13,21,22,23,31,32,33,41,42,43 };
```

- highest digit are **left most digits**
  - Make this more **readable**

```c++
    int four_by_three[4][3] = { {11,12,13}, {21,22,23}, 
                                {31,32,33}, {41,42,43} };
```

- Using an empty **brace** $\{\}$ inits some section to **hold all 0's**.

```c++
    int four_by_three[4][3] = { {11,12,13}, {}, {31,32,33}, {41,42,43} };
```

- Increasing dimensions $\to$ the **principle applies**
  - Increasing the **nesting** for the **right most dimension**.

```c++
    int four_by_three_by_two[4][3][2]  
       = { { {111,112}, {121,122}, {131,132} }, 
           { {211,212}, {221,222}, {231,232} }, 
           { {311,312}, {321,322}, {331,332} }, 
           { {411,412}, {421,422}, {431,432} }  
         };
```

- Rows columns and depth now.
- Access items **using same syntax**

```c++
std::cout << four_by_three_by_two[3][2][0] << std::endl; // prints 431
```

- For **memory layout**
  - Compiler interprets the **syntax** in the **following way** 
    1. The **first index** determines the **offset** from the **start** in chunks of **six integers** ($3 * 2$) 
    2. The **second index** is the **offset** one of these **six integer chunks** itself in **chunks** of **two integers**
    3. The **third index** is the **offset** in terms of **each integer**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210608142449888.png)

- A **multidimensional array** is treated as **array of arrays**
  - The **type of** each **row** is **$\text{int[3][2]}$** 
  - There are 4 of these as seen **from declaration** 

## Passing multidimensional arrays to Functions

- Pass **multi dimensional** array to **a function**

---

```c++
    // pass the torque of the wheel nuts of all wheels 
    bool safe_torques(double nut_torques[4][5]);
```

- **first dimension** $\to$ treated as **a pointer**.
  - Can pass a 4x5 or even 2x5, but not 4x3.
    - Not the same as the one **in the declaration**.

- Parameter essentially just **$\text{double row[][5]}$**
  - The **first dimension size** is *not available* 
    - The **function** should be *declared* with the **size of that dimension**.

```c++
    bool safe_torques(double nut_torques[][5], int num_wheels);
```

- nut torques is **one or more rows**
  - Where the rows must contain **5 columns**.
- Alternative declaration

```c++
  bool safe_torques(double (*nut_torques)[5], int num_wheels);
```

- Brackets are required
  - Without them $\to$ the $*$ refers to **the type of array**
    - This means it treats nut_torques as a **five element array** of **double*** pointers
    - This seen **before** as this **below**.

```c++
    void main(int argc, char *argv[]);
```

- **argv** parameter is **array** of **char*** pointers
  - Can declare the **argv parameters** as **char**** which has the **same meaning**

- Passing arrays $\to$ use **custom types**
  - Or use **C++ array types**
- Using the **ranged for** with **multidimensional arrays** is **more complex**
  - Requires use of a **reference** as explained **in later section**.

## Using arrays of characters

### Comparing Strings

- Following **allocates two string buffers** and calls **strcpy_s**
  - This **initializes** each with **the same string**.

```c++
    char p1[6]; 
    strcpy_s(p1, 6, "hello"); 
    char p2[6]; 
    strcpy_s(p2, 6, p1); 
    bool b = (p1 == p2);
```

- *strcpy_c* **copies characters** from the **pointer given** in the **last parameter** until the **terminating** **NUL** $\to$ into the **buffer given** within the **first parameter**
  - Where the **maximum size** is **given** in the **second parameter** 
- The **two pointers** are **compared** in the **final line**
  - This returns a **value of false**

- **Issue** is that **the compare function** is **comparing** the **values of pointers**
  - And **not** what the **pointers point to**
    - The **two buffers** have the **same string** but the **pointers** are **different ** so $b$ will be **false**.

---

- Correct **way** to **compare strings** is to **compare** the **data character** by **character** to check if they are **equal**
  - C **runtime** provides **strcmp** that **compares** the **two string buffers** *character by character*

- The **std::string** class defines a **function** called **compare** that perform said **comparison**
  - be cautious of **the value returned** from *these functions*.

```c++
    string s1("string"); 
    string s2("string"); 
    int result = s1.compare(s2);
```

- Return is **an int**
  - Carry out **lexicographical** comparison and **return negative value** if the **parameter**
  - s2 > **greater than** s1 *lexicographically* and a **positive number** if the **operand** is **greater than parameter**.
    - If **two strings** are the **same** $\to$ return 0
    - Remember **that** a **bool** is **false** for a **value** of $0$ and **true** for **non zero values**
- **Standard library** provides an **overload** for **== operator** of **std::string**, safe to **write code like this**.

```c++
    if (s1 == s2) 
    { 
        cout << "strings are the same" << endl; 
    }
```

- The **operator** will **compare the strings** contained in the **two variable**.

## Preventing Buffer Overruns

- The **C runtime library** for **manipulating strings** common for **allowing buffer overruns**

---

- The **strcpy** copies **one string to another**
  - Obtain **access** to this **through** the **<cstring>** header
    - Included in the **iostream** header $\to$ possible tempted to **write something like this**.

```c++
    char pHello[5];          // enough space for 5 characters 
    strcpy(pHello, "hello");
```

- Copy **six** to **five** 
  - May be taking from **user input** $\to$ assume the **allocated** is **big enough** however a **malicious user** could **provide** an **excessively** *long string deliberately* **bigger** than the **buffer** $\to$ overwrites a **part of the program**.
- These have been used by **hackers** to control **servers**
  - So much that **C string functions** have *all been replaced **by safer versions***
    - **Indeed** if you are **tempted** to **type preceding code** $\to$ find that **strcpy** is available.

```c++
error C4996: 'strcpy': This function or variable may be unsafe. 
Consider using strcpy_s instead. To disable deprecation, use _CRT_SECURE_NO_WARNINGS. See online help for details.
```

- If **you have existing code** that uses **strcpy**
  - You need to **make** it so the **code compile** $\to$ can **define** the **symbol** before **<cstring>**

```c++
#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
```

- An **initial attempt** to *prevent* this **issue** is **to call** *strncpy* 
  - Copy a **specific number of characters**

```c++
    char pHello[5];             // enough space for 5 characters 
    strncpy(pHello, "hello", 5);
```

- Function **copy up to five characters** and then **stop**
  - The **string copy** has *five characters* as a result there **will** be **there** no **NUL** termination.
    - The **safer function** has a **parameter** you can use to **say how big the destination buffer is**

```c++
    size_t size = sizeof(pHello)/sizeof(pHello[0]); 
    strncpy_s(pHello, size, "hello", 5);
```

- This **still causes** a **problem** 
  - You have **told the function** the *buffer* is **five characters** in **size**
    - Determines it **not big enough** to hold the **six characters** that you **request**.

- Calls the **constraint handler** and the **default version** will **shut down the program** on the **idea** that a **bugger overrun** means that the **program is compromised**
  - The **C runtime library** strings functions are **written** to **return** the **result of function**
    - The **safer versions** return an **error value**
- strncpy_s function can also **hold** to **truncate the copy** rather than **call constraint handler**

```c++
    strncpy_s(pHello, size, "hello", _TRUNCATE);
```

- C++ string class protects from said issues.

## Using Pointers C++

### Out of Bound Accessing

- When **allocating a buffer**
  - Whether on the **stack** or the **free store** $\to$ and **obtain a pointer**
  - Little prevention of **accessing out of bounds**
- When using **pointer arithmetic** / **indexed access** on **arrays**
  - Check **carefully** you are not **going out of bounds**
    - Sometimes the **error** may **not be immediately obvious**

```c++
    int arr[] { 1, 2, 3, 4 }; 
    for (int i = 0; i < 4; ++i)  
    { 
        arr[i] += arr[i + 1]; // oops, what happens when i == 3? 
    }
```

## Pointers to Deallocated Memory

- Following **poorly written** that returns a **string allocated** on the **stack** in some **function**

```c++
    char *get() 
    { 
        char c[] { "hello" };
        return c;
    }
```

- Allocates a **buffer of 6 characters** and **then initializes** it **with the five characters** of the **string literal** *hello*
  - The **NULL termination character** of course included
- When the **function ends** $\to$ *stack frame* is **torn**
  - As so **memory is reused** $\to$ the **pointer** will be **pointing somewhere else**.
    - This is **retarded programming**.
- Hard to notice if there are **several pointers** and **pointer assignments**.
  - May **not immediately** *notice* you return a **pointer** from **stack allocated object**.
- Best course is to **never return raw pointers**
  - To use this style $\to$ ensure the **memory buffer** is *passed as a parameter* (do **not give ownership** to the **temporary function**).
  - Or **dynamically allocating** and **passing ownership** to **said caller**.

---

- Leads to **further issues**
  - Calling **delete** on a **pointer** then **trying to access**
    - Potential access to **other variables being used**.
- Obtain a **habit** of  setting it to **null_ptr** when **delete** is *called* 
  - Then **check** for *nullptr* *before* using some pointer
- You can use a **smart pointer** which does this for you.

## Constant Pointers

-  Pointers can be declared as const which, depending on where you apply it, means that the memory the pointer points to is read-only through the pointer, or the value of the pointer is read-only:

https://stackoverflow.com/questions/2148769/typedef-and-containers-of-const-pointers

```c++
    char c[] { "hello" }; // c can be used as a pointer 
    *c = 'H';             // OK, can write thru the pointer 
    const char *ptc {c};  // pointer to constant 
    cout << ptc << endl;  // OK, can read the memory pointed to 
    *ptc =  'Y';          // cannot write to the memory 
    char *const cp {c};   // constant pointer 
    *cp = 'y';            // can write thru the pointer 
    cp++;                 // cannot point to anything else
```

- Here, ptc is a pointer to constant char, that is, although you can change what ptc points to, and you can read what it points to, you cannot use it to change the memory. 
- On the other hand, cp is a constant pointer, which means you can both read and write the memory which the pointer points to, but you cannot change where it points to. 
- It is typical to pass the const char* pointers to functions because the functions do not know where the string has been allocated or the size of the buffer (the caller may pass a literal which cannot be changed). 
- Note that there is no const* operator so char const* is treated as const char*, a pointer to a constant buffer.

- You can make a pointer constant, change it, or remove it using casts. The following does some fairly pointless changing around of the const keyword to prove the point:

```c++
    char c[] { "hello" }; 
    char *const cp1 { c }; // cannot point to any other memory 
    *cp1 = 'H';            // can change the memory 
    const char *ptc = const_cast<const char*>(cp1); 
    ptc++;                 // change where the pointer points to 
    char *const cp2 = const_cast<char *const>(ptc); 
    *cp2 = 'a';            // now points to Hallo
```

- The **pointers cp1 and cp2** can be used to **change the memory they point to**, but once assigned neither can point to other memory. 
- The first **const_cast** casts away the **const-ness** to a pointer that **can be changed** to point to other memory, but cannot be used to alter that memory, ptc. 
- The second const_cast casts away the const-ness of ptc so that the memory can be changed through the pointer, cp2.

## Changing the Type Pointer To

- **static cast** $\to$ used to **convert** with a **compile time check**
  - But **not runtime check**
  - Therefore the **pointer** must be **related**.
- **void*** $\to$ converted to **any pointer**
  - Therefore this **makes sense**

```c++
    int *pi = static_cast<int*>(malloc(sizeof(int))); 
    *pi = 42; 
    cout << *pi << endl; 
    free(pi);
```

- $C$ **malloc** returns **void*** $\to$ therefore must **convert** to use the memory.
  - C++ **new operator** *cuts* the **requirement** of **needing this**.
- Built in **types** are **not related enough** for *static_cast* to **convert** between **each pointer type**
  - Can **not use** the *static_cast* to **convert** an **int*** *pointer* to some **char*** *pointer*
    - Even though **int / char** are **both integer types** 

- For **custom types** that are **related** *through* inheritance 
  - Cast through **static_cast**, however there is **no runtime check** that the **cast is correct**
    - To cast with **runtime checks** $\to$ use **dynamic_cast**

https://stackoverflow.com/questions/332030/when-should-static-cast-dynamic-cast-const-cast-and-reinterpret-cast-be-used

---

- The **reinterpret_cast** is the **most flexible** / **dangerous** 
  - Converts between **any pointer types** with **no type check** - *INHERENTLY UNSAFE*
- The **following code** will *initialize* a **wide character** with some **literal**
  - The array of **wc** will have **six characters **
    - Hello  then **NULL**
- **wcout** *object* interprets a **wchar_t*** pointer as a **pointer** to the **first character** in a **wchar_t** string
  - Therefore inserting **wc** will **print the string** (*every character until NUL*)
- To obtain **the actual memory location** $\to$ have to **convert the pointer to an integer.**

 ```c++
     wchar_t wc[] { L"hello" }; 
     wcout << wc << " is stored in memory at "; 
     wcout << hex; 
     wcout << reinterpret_cast<int>(wc) << endl;
 ```

https://stackoverflow.com/questions/6384118/what-does-the-l-in-front-a-string-mean-in-c

- Inserting a **wchar_t** into the **wcout** object
  - Print the **character** and **not the numeric** *value*
- To **print out the codes** for the **individual characters**
  - We must **cast the pointer** to some **suitable integer** *pointer*
    - This **code** assumes that a **short** is the **same** *size* as **wchar_t**

```c++
    wcout << "The characters are:" << endl; 
    short* ps = reinterpret_cast<short*>(wc); 
    do  
    {  
        wcout << *ps << endl;  
    } while (*ps++);
```

## Allocating Individual Objects

- The **new operator** used with **type** to **allocate memory**
  - Return a **typed pointer** to **that memory**

```c++
    int *p = new int; // allocate memory for one int
```

- **new** $\to$ calls the **default constructor** for **custom types** for *every object* it **creates**

- **built in types** have *no constructors* , therefore some **type initialization** will *occur*
  - This often sets the **object to zero** (*in this example* $\to$ a **zero integer**).

---

- Should **not use memory** allocated for **built in types** without **explicitly initializing** 
  - Debug version of **visual studio** of the **new operator** will **initialize** memory to **a value** of $\text{0xcd}$ for **every byte**.
  - Visually remind that you have **not initialized memory**
- **Custom types** $\to$ left to **the author** of the **type** to **initialize allocated memory**

---

- When you have **finished memory** $\to$ return back to the **free store** so the **allocator** *reuse* 
  - You **do this by** *calling* the **delete operator**.

```c++
delete p;
```

- When **deleting a pointer**
  - The **destructor** of some object is **called**
    1. Built in types $\to$ this **does nothing**

>Good **practice** to **initialize** a **pointer** to **nullptr** after **deleting** 
>
>- If you use **convention** of **checking** the *value of a pointer* **before using it** $\to$ protect from **using a deleted pointer**

- C++ standard $\to$ states that *delete* operator have **no effect** if you **delete a pointer** of value **nullptr**.

---

- Can **init** a **value** at **the time** you **call the new operator**, in **two ways**:

```c++
int *p1 = new int (41);
int *p2 = new int {42};
```

- For some **custom type**
  - The **new operator** will *call a constructor* on the **type**
- For **built in type**
  - The **end** is the same and **carried** out by **initializing** the item to the **value provided**
- Can use **initialized list syntax** as **shown in the second line**
  - Important note is the **initialization** is the **memory** *pointed to* and **not the pointer variable**.

## Allocating Arrays of Objects

- Can also **create** some **arrays of objects** in **dynamic memory** using the **new operator**
  -  Provide the **number of items** you want **created** in a **pair of square brackets**

```c++
    int *p = new int[2]; 
    p[0] = 1; 
    *(p + 1) = 2; 
    for (int i = 0; i < 2; ++i) cout << p[i] << endl; 
    delete [] p;
```

- Operator **returns** a **pointer** to the **type allocated**.
  - Can use **pointer arithmetic** or **array index** to **access memory**.
- You can **not initialize** memory in the **new statement** 
  - Must do this **after creating the buffer**
- Using **new** to **create a buffer** for *more than one object*.
  - Must use **the appropriate version** of the **delete operator**.
    - $\text{[ ]}$ used to **indicate** *more than item* is deleted and the **destructor** for **each object** will **be called**

> Important to use the **appropriate** version **of delete** to the **version** of **new** used to **create the pointer**.

- **Custom types** can *define their own operator* **new** and *operator* **delete** for **individual objects** as well **as operator** new[] and **operator** delete[] for **arrays of objects**.
- The **custom type** author can use to **custom memory** *allocation* **schemes** for **their objects**.

## Handling failed allocations

- If **new operator** can **not allocate** the **memory** for some **object**
  - Throws **std::bad_alloc** exception and the **pointer returned** is a **nullptr**
- Vital to **check for failure** to **allocate memory** in **production code**
  - Following **code** shows how to **guard the allocation** so you can **catch** the **std::bad_alloc** *exception* and **handle it**

---

```c++
    // VERY_BIG_NUMER is a constant defined elsewhere 
    int *pi; 
    try 
    { 
        pi = new int[VERY_BIG_NUMBER]; 
        // other code 
    } 
    catch(const std::bad_alloc& e)  
    {  
        cout << "cannot allocate" << endl;  
        return; 
    } 
    // use pointer 
    delete [] pi;
```

- Any **code** in the **try block** throws **an exception** *control* it is **passed** to the **catch clause**
  - **ignoring** any **other code** that has **not been executed yet**.
- The *catch* clause **checks** the **type** of the **exception object** and if its the **correct type**
  - this case $\to$ its an **allocation fault**
  - Creates a **reference** to **that object** and *passes control* to the **catch block** 
    - The **scope** of the **exception** *reference* in this **block**

- For this **example** $\to$ the **code merely** prints **an error**
  - You would use **it to take action** to **ensure** that the **memory allocation** does **not affect subsequent code**

## Other versions of the new operator

- A **custom type** can **define** a *placement operator* **new**
  - Enables you to **provide** *one or more* **parameters** to the **custom new function**
    - The *syntax* of the **placement** *new* is to provide the **placement fields** via **parentheses**.
- C++  *standard library* version of **new** gives **a version** that can **take ** the **constant** *std::nothrow* as **placement field**
  - This version does **not throw an exception** if an **allocation fails**
    - Rather , its only **assessed** from the **value of the returned pointer**.

```c++
    int *pi = new (std::nothrow) int [VERY_BIG_NUMBER]; 
    if (nullptr == pi)  
    { 
        cout << "cannot allocate" << endl; 
    } 
    else 
    { 
        // use pointer 
        delete [] pi; 
    }
```

- The **parentheses** before the **type** are used to **pass placement fields**.
  - If you use them **after the type** $\to$ you give a value to **initialize** the **object** if **successful allocation**.

## Memory Lifetime

- Memory allocated by **new** $\to$ remain *valid* until **calling delete**
  - May have memory with **long lifetimes** and the **code may be passed** around *various functions* within the code
    - Consider:

https://stackoverflow.com/questions/47366453/direct-initialization-vs-direct-list-initialization-c - direct vs list 

https://stackoverflow.com/questions/50422129/differences-between-direct-list-initialization-and-copy-list-initialization/50422189

https://en.cppreference.com/w/cpp/language/initialization

http://mikelui.io/2019/01/03/seriously-bonkers.html

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210609185608990.png)

```c++
    int *p1 = new int(42); // creates a pointer and initializes memory
    int *p2 = do_something(p1); // passes a pointer to some function, of which returns a pointer
    delete p1; // no longer required, therefore deleted and set nullptr
    p1 = nullptr; 
    // what about p2?
```

- What if the **function** just **manipulates** the data **pointed to by the pointer**

```c++
    int *do_something(int *p) 
    { 
        *p *= 10; 
        return p; 
    }
```

- This in effect $\to$ creates a **copy of  a pointer**
  - But does **not copy** what the pointer **points to**
    - Therefore when **p1 is deleted** $\to$ the memory it **points to ** is **no longer available** therefore **p2** points to **invalid memory**.
- Addressed via a **mechanism** of **Resource Acquisition Is Initialization** RAII
  - Using features of **C++ OBJECTS** to **manage resources**
  - RAII requires classes $\to$ in particular , **copy constructors** and **destructors** 
  - A **smart pointer class** can used to **manage a pointer** so that **when its copied**
    - the memory its **pointed to** is **copied also**
- **destructor** $\to$ a **function** that is **called automatically** when the **object** goes **out of scope** 
  - Therefore a **smart pointer** uses this to **free memory**
    - Smart pointers / destructors will **be covered** 

---

## Windows SDK and Pointers

- Returning **pointer** from a **function** has danger
  1. The **responsibility** of the **memory** is **passed to caller**
  2. Caller must ensure **correct deallocation** of the **memory**
     - Cause **memory leak otherwise** which would **cause loss of performance**.

---

- A **function** in the **windows SDK** that *returns **string*** / **has string parameter** 

  - Comes in **two versions**

    1. Version with **suffix** -A indicates it uses **ANSI strings**

       https://stackoverflow.com/questions/701882/what-is-ansi-format

    2. W - version  uses **wide character strings**

  - Using ANSI strings in this case.

---

- **GetCommandLineA** function has **the following prototype**

```c++
char * __stdcall GetCommandLine();
```

- Every **windows functions** are defined using the **__stdcall** *calling convention*
  - Often see the **typedef** of **WINAPI** used for **__stdcall** calling convention.

---

- Function can be **called like this**:

```c++
    //#include <windows.h>
    cout << GetCommandLineA() << endl;
```

- Nothing in **freeing** the *returned buffer*
  - This pointer points to **memory** that **lives the lifetime** of *your process* therefore **do not release it.**
  - Hard to delete as you **cant guarantee** the **compiler** used to **return the pointer**.

---

- When functions **return buffers** $\to$ always **consult documentation** to check **who allocated** the **buffer** and who **should release it**
  - Another example $\to$ **GetEnvironmentStringsA** 

```c++
    char * __stdcall GetEnvironmentStrings();
```

- Returns a **pointer** to a **buffer**.
  - This **time** $\to$ documentation states to **release** after **using the buffer**.
- SDK provides a **function** to do this $\to$ called **FreeEnvironmentStrings** 
  - Buffer contains **single string** for *every environment* of form **name = value**
  - Each string is **terminated** by a **NUL character**
    - *Last* string in **buffer** is a **NUL character** 
- There are **two NUL characters** at the **end of buffer**
  - Functions as **used like this**:

```c++
    char *pBuf = GetEnvironmentStringsA(); 
    if (nullptr != pBuf) 
    { 
        char *pVar = pBuf; 
        while (*pVar) 
        { 
            cout << pVar << endl; 
            pVar += strlen(pVar) + 1; 
        } 

        FreeEnvironmentStringsA(pBuf); 
    }
```

- **strlen** is part of **C runtime library** and **returns** the **length of a string**.
  - Do **not need** to know how the **GetEnvironmentStrings** function *allocate* the **buffer** as the **FreeEnvironmentStrings** calls the **correct deallocation code**.

---

- Cases when the **developer** has the **responsibility** of **allocating a buffer**
  - Windows **SDK** has a function called **GetEnvironmentVariable**  to *return* the **value** of a **named environment variable**
    - When **calling** this **function** $\to$ you **do not know** if the **environment variable** is *set* or **if its set** or **how big the value is**
      - Likely have to **allocate some memory**
- The **prototype** of the **function**

```c++
    unsigned long __stdcall GetEnvironmentVariableA(const char *lpName,   
        char *lpBuffer, unsigned long nSize);
```

- There are **two parameters** that are **pointers** to **C strings**
  - Problem $\to$ a **char*** pointer could be **passing in** a **string** to **the function**
    - Alternatively used to **pass in a buffer** for a **strings** to be **returned out**.
- How can you **know** what **char* pointer** is **intended** to be **used for**.

---

- Clue with **full parameter declaration**
  - The **lpname** *pointer* marked **const** therefore it **does not alter** what the **string is** that it **points to**.
    - Therefore its an **in parameter**.
    - Used to **pass** in the **name of the environment variable** that you **wish to obtain**.

- The **other parameter** is just **char*** *pointer* 
  - Could be **used to pass a string in** to the **function** or **even out** or **both**.
- Only way to **know** *how* to **use this parameter** is to **read the documentation**
  - For this case $\to$ its an **out parameter** $\to$ the **function** will **return** *value* of the **environment variable**  $\text{lpbuffer}$ if the **variable exists** 
  - Otherwise **leave untouched** if the **variable does not exist** and return **value 0**.

- Its **your responsibility** to *allocate* a **buffer** in *whatever* way **you see fit**
  - and you must **pass the size** of **this buffer** in the **last parameter** of **nsize**.

---

- The **function** *return value* has **two purposes**
  - Indicates an **error** has **occurred** $\to$ just *one value* of $0$ $\to$ you have to **call the** **GetLastError** function to **get the error**
    - Used to **give information** on the **buffer** $\text{lpbuffer}$ 
- If **successful** $\to$ return value is the **number of characters** *copied* into the **buffer** 
  - It **excludes** the **NULL** *terminating* character
- If the **function** determines the **buffer too small** (it **knows** this from **nsize**) to **hold environment variable** *value*
  - Then **no copy will happen** and the function **returned** *required* **size of the buffer**
    - This is the **number of characters** in the **environment variable** including **the NUL terminator**.

---

- Common method is **to call twice**
  - First with **zero sized buffer** $\to$ use the **return value** to *allocate a buffer* **before** *calling it again*.

```c++
    unsigned long size = GetEnvironmentVariableA("PATH", nullptr, 0); 
    if (0 == size)  
    { 
        cout << "variable does not exist " << endl; 
    } 
    else 
    { 
        char *val = new char[size]; 
        if (GetEnvironmentVariableA("PATH", val, size) != 0) 
        { 
            cout << "PATH = ";
            cout << val << endl; 
        } 
        delete [] val; 
    }
```

- **Good to read documentation** to *determine* how the **parameters** are **used**.
  - The **windows documentation** tells you if some **pointer parameter** is **in, out , in/out**.
- *Tells* you **who owns memory** and *whether* you have **responsibility** for *allocating* and/or **freeing the memory**

---

## Standard Library Arrays (Memory and the C++ Standard Library)

- The C++ Standard Library provides two containers that give random access via an indexer to the data. 
- These two containers also allow you to access the underlying memory and since they guarantee to store the items sequentially and contiguous in memory, they can be used when you are required to provide a pointer to a buffer. 
- These two types are both templates, which means that you can use them to hold built-in and custom types. These two collection classes are array and vector. 

## Using stack based array class

- **array** defined in the **<array>** *header file*
  - Class $\to$ enables you to **create fixed sized** *arrays* on the **stack** 
  - Can **not shrink** or **expand** at the **runtime**
- Allocated on the **stack** $\to$ do *not require memory call* **allocator** in the **runtime**
  - They should be **smaller** than the **stack frame size**

> Ideal for **smaller arrays of items**

- The **size** of **an array** must be **known at compile time**
  - Passed as a **template parameter**:

```c++
    array<int, 4> arr { 1, 2, 3, 4 };
```

1. Template **parameter** $1$ in **angle brackets** (< >) is the **type**
2. The second is **how many items to allocate**.

---

- This code **initializes** with an **initialize list**.
  - Still have to **provide the size** of the **array** in the **template**.
    - This object work with **like built in array** with **ranged for** ( like any of **standard library containers**).

```c++
  for (int i : arr) cout << i << endl;
```

- The **array** actually **implements** the **begin / end** functions that are required for **this syntax**
  - Also use **standard method**.

```c++
    for (int i = 0; i < arr.size(); ++i) cout << arr[i] << endl;
```

- Size returns array size
  - Can **access** *beyond* the **bounds** but causes **unexpected runtime results** or potentially a **memory fault**
    - Guard this via use **of at** $\to$ performs **range check** and calls **C++ exception** *out_of_range*.

---

> The **main advantage** of using **array object** is that you get **compile-time checks** to check if you are **inadvertently** passing **object** to a **function** as a **dumb pointer** (basic pointer)

- C++ allows a **built-in array** to be **used a pointer**.

```c++
int arr[] {1,2,3,4};
use_ten_ints(arr1); // read past the end of the buffer
```

- There is **no compiler check** or **runtime check** to *catch this error*
  - The **array class** will **not let this error** happen as there is no **automatic conversion** into some **dumb pointer**

```c++
    array<int, 4> arr2 { 1, 2, 3, 4 };  
    use_ten_ints(arr2); // will not compile
```

- Insist in **obtaining a dumb pointer**
  - Can be done and **be guaranteed** to have **access** to the **data** as some **contiguous block of memory** where the items are stored **sequentially**

```c++
    use_ten_ints(&arr2[0]);    // compiles, but on your head be it 
    use_ten_ints(arr2.data()); // ditto
```

- Class is **not just a wrapper** around some **built in array**
  - Provides some **additional functionality**

```c++
    array<int, 4> arr3; 
    arr3.fill(42);   // put 42 in each item 
    arr2.swap(arr3); // swap items in arr2 with items in arr3
```

## Using the Dynamically allocated vector class

- The Standard Library also provides the vector class in the <vector> header.
-  Again, this class is a **template**, so you can use it with **built-in and custom types**. However, unlike array, the **memory is dynamically allocated,** which means that a vector can be expanded or shrunk at runtime.
-  The items are **stored contiguously** so you can **access the underlying buffer** by calling the **data function** or accessing the address of the first item (to **support resizing the collection**, the **buffer may change**, so such pointers should only be used temporarily). And, of course, as with array, there is no automatic conversion to a dumb pointer. 
- The vector class provides **indexed random access** with **square bracket syntax** and a range check with the at function. The class also implements the methods to allow the container to be used with **Standard Library functions and with ranged for.**

---

- The vector class has **more flexibility** than the array class because you can **insert items, and move items** around, but this does come with some **overhead**. 
- Because instances of the class **allocate memory dynamically** at runtime there is a cost of **using an allocator** and there is some extra **overhead in initialization** and **destruction** (when the **vector object goes out of scope**).
-  Objects of the vector class also **take more memory** than the **data it holds**. For this reason, it is not suitable for small numbers of items (when array is a better choice).

## References

- Reference just an **alias to an object**
  - Its **another name** for **some object** and so **access** to the **object** is the **same** via **reference** as its via the **variable name**
- Declared using the **&** symbol on the **reference name**.
  -  Its **initialized** and **accessed** in **the same way** as a **variable**.

```c++
    int i = 42; 
    int *pi = &i;  // pointer to an integer 
    int& ri1 = i;  // reference to a variable 
    i = 99;        // change the integer thru the variable 
    *pi = 101;     // change the integer thru the pointer 
    ri1 = -1;      // change the integer thru the reference 
    int& ri2 {i};  // another reference to the variable 
    int j = 1000; 
    pi = &j;       // point to another integer
```

- A **variable declared** and **initialized**.
- A **pointer** then **initialized** to **point** to **this data**.
- A **reference** is **initialized** as an **alias** for **variable**.
- Reference **ri1** is **initialized** with **an assignment operator**.
- **ri2** initialized using **initializer list syntax**.

>The *pointer* and *reference* have **two different meanings** 
>
>Reference is **not initialized** to the **value of the variable** $\to$ its **data**
>
>its just an **alias** for the **variable name**. 

- Wherever **the variable is used** $\to$ the **reference can be used** 
  - Whatever you do to the **reference** is the **same as performing** the *same operation on the variable*
- A **pointer** just **points** to data so you **can change the data** by **dereferencing** the *pointer*
  - illustrated in the **last two lines** of the **preceding** *code*.
- Can have **several aliases** for **some variable**
  - Each must be **initialized** to the *variable* at its **declaration**.
- Once **declared** cannot **make a reference** *refer* to some **different object**.

---

```c++
    int& r1;           // error, must refer to a variable 
    int& r2 = nullptr; // error, must refer to a variable
```

- It **cannot exist** (references) without being **initliazed** to some **other variable**
  - Cannot **init** to **anything** other than **a variable name**.
    - There is **no concept** of a **null reference**.

---

- A reference is only **ever an alias** to **one variable**
  - When you use a **reference** as an **operand** to any **operator** $\to$ performed on **said variable**.

```c++
    int x = 1, y = 2;  
    int& rx = x; // declaration, means rx is an alias for x 
    rx = y;      // assignment, changes value of x to the value of y
```

- $rx$ is an **alias** to the **variable** $x$ 
  - Therefore the **assignment in the last line** *simply assigns* $x$ with the **value** of $y$ 
    - The **assignment** is **performed** on the **aliased variable** 
    - If you take the **address** of a **reference** $\to$ returned the **address of variable** it **references**
- You can have a **reference** to an **array**
  - Cannot have **array of references**.

## Constant References

- So far you can **change the variable** its an **alias for**
  - Thus has **lvalue** *semantics*
- Can have **const lvalue references** $\to$ reference to an object that you can / **not write**.

---

- Like **const pointers** $\to$ you declare **const reference** using **const** on some **lvalue reference**

```c++
    int i = 42; 
    const int& ri = i; 
    ri = 99;           // error!
```

## Returning References

- Sometimes an object will be passed to a function and the semantics of the function is that the object should be returned.
-  An example of this is the **<< operator** used with the stream objects. Calls to this operator are *chained*:

```c++
    cout << "The value is " << 42;
```

- This is a variety of calls to **functions** called **operator<<**
  - One takes a **const char*** *pointer* , another takes an **int parameter**.
- The functions also have an **ostream parameter** for the **stream object** that *will be used*
  - If its just a **ostream parameter** then it would **mean** that a **copy of parameter** would **be made** and the **insertion** would **be performed** on **the copy**.
- **Stream objects** often **use buffering**
  - *changes* to a **copy of a stream object** may **not have the desired effect**
- Enable **chaining of insertion operators**
  - The *insertion*  functions **return** the **stream object passed** as a **parameter**
    - In the **following code** a **reference is returned** to an **automatic variable**

```c++
    string& hello() 
    { 
        string str ("hello"); 
        return str; // don't do this! 
    }   // str no longer exists at this point
```

- For **preceding code** $\to$ the **string object** only lives as **long as the function**
  - so the **reference** *returned by this function*  will **refer** to an **object that does not exist**
    - Of **course** $\to$ you can **return** a **reference** to a **static variable** *declared in a function*.
- **Returning** a **reference** from a **function** is a **common idiom**
  - Whenever you **consider** doing $\to$ ensure the **lifetime** of the **aliased variable** is not the **scope of the function**

## Temporaries and References

- The **lvalue reference** must **refer** to a **variable**
  - C++ $\to$ has some **odd rules** when it **comes to const** *references* *declared* on the **stack**
    - If the **reference** is a **const** $\to$ the **compiler** will extend the **lifetime** of a **temporary** for the **lifetime of the reference**

- If you use **initialization list syntax** $\to$ compiler will **create a temporary**.

```c++
    const int& cri { 42 };
```

- In **this code** $\to$ compiler create a **temporary** *int* and **initialize** to a **value** 
  - Then **alias** to the **cri reference** $\to$ (**it is important**) that this **reference is const**)
- The **temporary** is *available* through the **reference** while **in scope**
  - This may **look odd** $\to$ consider using a **const** reference in **this function**.

```c++
    void use_string(const string& csr);
```

- Can **call this function** with a **string variable**
  - A **variable** that will **explicitly** convert to a **string** or with a **string literal**.

```c++
    string str { "hello" }; 
    use_string(str);      // a std::string object 
    const char *cstr = "hello"; 
    use_string(cstr);     // a C string can be converted to a std::string 
    use_string("hello");  // a literal can be converted to a std::string
```

- In most cases, you'll **not want to have a const reference** to a **built-in type**, but with **custom types** where there will be an overhead in making **copies** there is an **advantage** and, as you **can see here**, the **compiler will fall back to creating a temporary if required**.

## The **rvalue reference**

- New in **C++11** defines a **new type of reference** $\to$ **rvalue**
  - *Before C++11* $\to$ there was **no that code** (*like an assignment operator*) could **tell** if some *rvalue* **passed** to it was a **temporary** *object* or **not**
- If such function $\to$ **passed** a **reference** to an **object** $\to$ then the **function** has to be **careful not to change** the **reference** as this **affects** what the **object refers to**
  - If the **reference** is to a **temporary object** $\to$ the function can **do what it likes** to the **temporary object** as the **object** will **not live** after the **function completes**.

- C++11 $\to$ enables you to write **code** that is **for temporary objects** $\to$ for **assignment**
  - The **operator for temp objects** can just **move data** from the **temp** into the **object being assigned**
    - Contrast $\to$ if the **reference** is **not to a temp** then the **data must be copied** $\to$ if **large** $\to$ **expensive allocation and copy**
- Therefore enabling **move semantics** 

```c++
   	// 3 functions that return a string object

	// case 1 > string has the lifetime of the program therefore a reference can be returned
	string global{ "global" }; 

    string& get_global() 
    { 
        return global; 
    } 

	// case  2 > same concept as case 1 

    string& get_static() 
    { 
        static string str { "static" }; 
        return str; 
    } 

	// returns a string literal, so a temporary string object is constructed
    string get_temp() 
    { 
        return "temp"; 
    }

	// all three can be used to provide a string value

    cout << get_global() << endl; 
    cout << get_static() << endl; 
    cout << get_temp() << endl;

	// all three provide a stirng that can be used to assign a string object
 	// important point is the first two functions return along a lived object
	// Third function returns a temp object, but these objects can be used the same 
```

- If the functions **returned access** to some **large object** $\to$ you would **not want to pass the object** to some **other function**
  - In **most cases** $\to$ you will want to **pass the objects** returned by **these functions** as **references**, example:

```c++
    void use_string(string& rs);
```

- **Reference parameter** $\to$ *prevents* a **copy of string**
  - The **use_string** function may **manipulate said string**
- Function

```c++
// creates a new string from the paremeter, replaces the letters a,b, and o with an underscoe

/* string object has index operator [] > treat like an array of character
	Both reading values of characters and assigning values to character positions
	The size of the string is obtained via length > returns unsigned int (typedef to size_t)
	
	The parameter is a reference > any change to the string is reflected in the original string passed in 
	intention is  to leave the other variable the smame therefore makes a copy of the parameter
	ON the copy > the code iterates through all the characters changing the a,b and o  characters to an underscore before 		printing result
	

*/
void use_string(string& rs) 
    { 
        string s { rs }; 
        for (size_t i = 0; i < s.length(); ++i) 
        { 
            if ('a' == s[i] || 'b' == s[i] || 'o' == s[i])  
            s[i] = '_'; 
        } 
        cout << s << endl; 
    }

/*
	clear copy overhead
	creating the string (s) from reference (rs) > this is required if we wish to pass strings like thjose from get_global
	or get_static to this function as otherwise changes would be made to the actual global  and static variables

*/

/*
	However the temporary string returned from get_temp is another situation
	This temp object only exists till the end of the statement that calls get_temp
	possible to make changes knowing it affects nothing else
	therefore you can use move semantics
*/

/*
	now use && to indicate the parameter is identifeid as an rvlaue reference
	
	other change > that changes are made on the object that the reference refers to as we know that its a temporary 
	and the changes will be discarded therefore not affect the other variables
*/
    void use_string(string&& s) 
    { 
        for (size_t i = 0; i < s.length(); ++i) 
        { 
            if ('a' == s[i] || 'b' == s[i] || 'o' == s[i]) s[i] = '_'; 
        } 
        cout << s << endl; 
    }

/*
	There are now two functions, overloads with the same name > one with lvalue reference and one with rvalue
	calling thsi function > compiler calls the right one according to the parameter that has been passed in.
*/

    use_string(get_global()); // string&  version 
    use_string(get_static()); // string&  version 
    use_string(get_temp());   // string&& version 
    use_string("C string");   // string&& version 
    string str{"C++ string"}; 
    use_string(str);          // string&  version
```

- get_global and **get_static** return **references** to **objects** that will **live the lifetime** of the **program**
  - For this reason $\to$ compiler choose the **use_string** version that takes **lvalue references**.
- Changes are made on a **temp variable** with **inside the function**
  - This has the **copy overhead present** 
    - The *get_temp* returns **temp object** and therefore the compiler calls the **overload** of **use_string** using **rvalue references** 
      - This functions **alters the object** that the **reference refers to** but this is **not an issue** as it  **doesn't last** *beyond its semicolon* at the end of the line $\to$ same said for **use_string** with some **c-like string literal.**

- Compiler $\to$ creates a **temporary string object** and *call the overload* that has an **rvalue reference** parameter
  - Final example $\to$ A c++ string object **created on stack** and **passed** to **use_string**
    - Sees its an **lvalue** and **can be altered possibly** $\to$ therefore **calls the overload** that **takes** an **lvalue reference** that is implemented in a way that **only alters** a **temporary local variable** in **the function**.

- This final example $\to$ shows the **C++ compiler** can *detect* when a **parameter** is a **temp object** and **call the overload with rvalue reference**
  - Often used in **copy constructor writing** $\to$ create **new custom type** from **existing instance** 
    - And also **assignment operators** so the functions can **implement** the **lvalue reference** *overload to copy data* from the **parameter** 
    - And the **rvalue reference overload** to **move the data** from the **temp** to a **new object**
- Others for **writing custom types** that are **move only** $\to$ where they use **resources** that **cannot be copied** such as **file handles**

---

## Ranged For and References

- An example **of what you can do with references**
  - **worth looking** at the **ranged for facility** in **C++11** *facility*.

- The **following code** is **quite straightforward** $\to$ array *squares* is **init** with **the squares** of $0$ to $4$:

```c++
	constexpr int size = 4; 
    int squares[size]; 

    for (int i = 0; i < size; ++i) 
    { 
        squares[i] = i * i; 
    }

```

- *Compiler* **knows the size of array**
  - can use **ranged for** to *print out the **values*** in the **array** 
- In **the following** on **each iteration**  $\to$ the *local variable* $j$ is a **copy** of the **item** in **the array**
  - As just a **copy** $\to$ means you **can read the value** , however **any changes** made to the **variable** are **not reflected in the array**
    - Therefore the **code here** works **just as expected** and *prints out the contents* of **the array**.

```c++
    for (int j : squares) 
    { 
        cout << j << endl; 
    }
```

- To **change values** $\to$ must declare as a **reference** in the **for ranged loop** *variable*.

```c++
    for (int& k : squares) 
    { 
        k *= 2; 
    }
```

- On **every iteration** $\to$ $k$ variable is an **alias** to an **actual member** in the **array**.
  - Whatever is done **with $k$** $\to$ actually **performed** on the **array member**.
    - In **the following example** $\to$ every *member* of the **squares array** is *multiplied* by $2$ 
- Cannot use **int*** $\to$ for **type** of $k$ as the **compiler** sees the **items** in array is **int** 
  - Uses this as the **loop variable** in the **ranged for** 
- A **reference** is just an **alias** $\to$ therefore you can **actually change the member**

---

- *Ranged for* $\to$ is **interesting for multidimensional arrays**
  - A **2D array** is *declared* and **attempt** made to **use nested loops** via the **auto variables**

```c++
    int arr[2][3] { { 2, 3, 4 }, { 5, 6, 7} };   
    for (auto row : arr) 
    { 
        for (auto col : row) // will not compile
        //this range-based 'for' statement requires a suitable "begin" function and none was foundC/C++(2291)//
        { 
            cout << col << " " << endl; 
        } 
    }
```

- This is an **array of arrays** $\to$ every row in itself is a **one dimensional array**
  - The **intention** is to **obtain each row** within the **outer loop** and then **inner loop** to *access each item*
    - There are **several issues** with *this approach* , it **will not actually compile however**

- Complains about **inner loop** $\to$ can **not find a begin** or **end function** for the *type* of **int*** 
  - This as the **ranged for** uses **iterator objects** and for **arrays** it uses **C++ standard library functions** *begin / end* to create these **objects**.
- Compiler see from the **arr** array in the **outer ranged**
  - For **each item** is an **int[3]** array $\to$ so for the **outer for loop** $\to$ loop variable is a **copy of each element** (int[3] array in this case).
    - Can **not copy arrays** in **this manner** $\to$ compiler **provides pointer** to the **first element** an **int***
      - Used in the **inner for loop**.

- Compiler **attempts** to **obtain iterators** for **int***
  - this is **not possible** as the **pointer** contains **no information** about **how many items it points to**
    - There is **begin and end ** for **int[3]** however **not for int***. 
- A **simple change** makes the **code compile**
  - Turn the **row** variable into **reference**.

http://www.cplusplus.com/forum/beginner/212616/

```c++
    for (auto& row : arr) 
    { 
        for (auto col : row) 
        { 
            cout << col << " " << endl; 
        } 
    }
```

- This **reference parameter** *alias* used for **int[3]** array
  - An **alias** is the **same as the element**
    - Use of **auto** hides the **ugly nature** of what it **defined as in its variable**.
- Outer loop is in fact **int (&)[3]**
  - It is a **reference** to an **int[3]** (*parentheses dictate that **it references an int[3]*** and not **array of int&**) 

## Pointers in Practice





  *Commmon requirement is to have a collection of arbitrary size that can grow / shrink at runtime*

  *C++ standard library has various classes enabling the ability to do this*

  

  *Following shows how said collections are implemented, should use standard templates rather than do this however*

  *Standard librarly classes encapsulate code together in a class, the following code assumes no use of classes so is not advised.*

  

  *Linked list common data structure* 



  *in this example > each task is represented as a structure that contains the task description and a pointer to next to be performed*



---

- If the pointer to the **next task** is **nullptr** then the **current task** is the **last task**

```c++
struct task{
    
    task *pNext;
    string description;
    
}
/*
	Access members of the strcture using the dot operator via an instance
*/

task item;
item.description = "do something you cunt";

/*
	The compile creates a string object init with the string ltieral do something
	assign to description member of the instance called item
	
	Can createa a task using the new operator
	
*/

task *pTask = new task;
// use object
delete pTask;

/*
	For this case > the members of the object must be accessed through some pointer
	C++ provides the -> operator to give this access

*/

task *pTask = new task;
pTask->description = "do something";
//use
delete pTask
    
    /*
    	The description is assigned to the string, as the task is a structure, there are no access restrictions
    	Something that is important with classes and described in chapter 4 classes later.
    */
```

