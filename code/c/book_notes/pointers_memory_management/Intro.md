# Introduction

- Pointer $\to$ variable that stores the **address** of a **memory location**

## Pointers and memory

- When a **c** program is **compiled** $\to$ it works with **three types** of **memory** 

#### Static / Global

- Statically declare variables are **allocated** to **this type of memory**
- **Global** variables also use this **region of memory**
  - They are *allocated* at the **start of the program** and *remain in existence* until **termination**
- **every function** can access **global variables**
  - The **scope** of *static variables* is **restricted** to its **defining function**.

---

#### Automatic

- These variables are **declared** within a **function** and are created when a **function is called**
  - The *scope* is *restricted* to the **function**
    - The **lifetime** is *limited* to the **time** the **function** is *executing*.

#### Dynamic

- Memory is **allocated** from the **heap** and *released* as **necessary**.
  - A *pointer* $\to$ **references** the *allocated memory*
    - The **scope** is *limited* to the **pointer/s** that *reference* that section of memory.
      - Its within **existence** until **released**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210415025830880.png)

- Understand the various types of memory $\to$ enables **better understanding** of *how* *most pointers actually work*
  - Most pointers $\to$ used to **manipulate** the **data** within **memory**.
    - understanding how memory is **partitioned** and **organized** will *clarify* how **pointers** are able to **manipulate memory**.

---

- A *pointer* variable $\to$ contains the **address** in **memory** of *another variable* / *object* / *function*
  - An **object** $\to$ considered to be **memory allocated** using on the *memory allocation functions* such as **malloc**.

---

- Pointer often declared as a **specific type** *depending* on *what it **points to*** 
  - Such as *pointer* to a **char**.
- The *object* $\to$ may be **any C** *data type* such as **integer / character / string / structure**
  - However there is **nothing inherent** in a **pointer** which indicates what **type of data** the actual **pointer** is **referencing**.
    - It just **contains** the **address**.

### Motive for using pointers to advanced level

- Create **fast / efficient** *code*
- Provide **convenient** method of **addressing** the *many types of problems*
- Supports **dynamic memory allocation**
- Making **expression** more *compact* and **succinct**
- Provide ability to **pass data structures** by *pointer* without **incurring** some **larger overhead**. 
- Protecting data passed as **parameter** to some **function**

---

- Code often **faster / efficient** using pointers as its **closer** to **hardware**
  - The **compiler** can *more easily translate* **the operation** to *machine code*
    - There is not **much overhead** that is *associated* with **pointers** as with **other operators**.

- Many **data structures** are better implemented using **pointers**
  - A *linked list* for **example** using either arrays or pointers.
    - Pointers are **easier** to **use** and **map directly** to a **next / prev** *link*
  - The **array implementation** requires the *array indexes* $\to$ these are **not** as *intuitive* or as **flexible** as **pointers**.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210415032405695.png)

- Figure above $\to$ shows the **visualistion**  using *arrays / pointers* for a **linked list** of *employees*
  - The **lefthand** side of the **figure** uses an **array**.
    - The *head variable* indicates the **linked list** begins at *index 10*. 

- Each array element contains structure that **represents employee**
  - The *structures* *next* field **holds** the *index* in the **array** of the **next employee** 
    - The **shaded elements** are the **free spaces**.
- The **righthand** *side* $\to$ shows the **equivalent representation** using *pointers*
  - The *head variable* $\to$ holds pointer to **first employee node**
    - Each *node* $\to$ holds **employee data** as well as a **pointer** to the **next** *node* within the **linked list**.

- The **pointer version** $\to$ is *clearer* and **more flexible**
  - The **size** of an *array* must be **known** within its creation.
    - This **restricts** *the* **number** of elements it **can hold**
- Pointer representation $\to$ does **not suffer** from this as a **new node** can be **dynamically** added as required.

---

- **Dynamic memory allocation** is effected in $C$ via use of **pointers**
  - The **malloc** and **free** are used to **allocate** and **release** *dynamic memory* *respectively.*

---

- **DMA** enables *variable sized arrays* and **data structures** 
  - Such as **linked lists** and **queues**

---

- **Compact expressions** are *very descriptive* but **hard to read**
  - As the **pointer notation** is not *always fully understood*. 
- They should be **useful** and just **cryptic**.
  - Following sequence $\to$ the **third character** of the *names'* second element is **displayed** with **two different** $printf$ functions.
    - Simpler approach just to use **array notation**

```c
char *names[] = {"Miller","Jones","Anderson"};
    printf("%c\n",*(*(names+1)+2));
    printf("%c\n",names[1][2]);
```

- Pointers are **powerful tools** to create and **enhance applications**
  - There are **various pointer** issues:

1. *Accessing arrays* and **other data structures** beyond their bounds
2. Referencing **automatic variables** after they *have gone out of existence*
3. Referencing **heap allocated memory** after its **released**. 
4. *Dereferencing* a **pointer** before **memory** has **been allocated** to *it.*

---

- Situations where the **specification** does *not explicitly* define **pointer behaviour**
  - These case $\to$ behaviour defined either as:

#### Implementation defined

- Some **specific** / **documented** *implementation* is **provided**
  - An **example** of **implementation** defined behaviour is how the **high order bit** (MSB) is *propagated* in **integer shift right operation**

#### Unspecified

- Some **implementation** is *provided* but **not documented**.
  - An **example** of an *unspecified* **behaviour** is the *ammount of memory* allocated by the **malloc** *function* with an **argument** of **zero**.

#### Undefined

- There are **no requirements**
  - Imposed and *anything can happen*
    - An **example** of this is the **value of a pointer** that is **deallocated** by  the *free function*. 
      - A list of unspecified behaviour can be found at CERT secure coding appendix CC.

---

- May be **locale specific behaviours**.

### Declaring Pointers

- Declared using a **data type** followed by **an asterisk** and then the **pointers variable name**
  - An **integer and  pointer to integer below**:

```c
int num;
int *pi;
```

- The use of **white spaces** $\to$ around the **asterisk is irrelevant** 
  - The following declarations are **all equivalents**.

```c
int* pi;
int * pi;
int *pi;
int*pi;
```

- The **asterisk** declares it as a pointer.
  - It can be **overloaded** $\to$ for **dereferencing** and **multiplications**.

---

- Figure below shows how memory above would be **allocated**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210415222243502.png)

- Address > block of memory.
- Three dots represent **uninitialized memory**

---

- Pointers to UNINIT memory can be an **issue**

  - If such pointer is **dereferenced** $\to$ the *pointers content* does **not represent** some **valid address**.
    - It may *not contain valid data*.

- Some **invalid address** is one of which the current program has **no authorization** to *access*
  - This results in **program termination.** 
    - This is important and can **lead to many problems**.

---

- variables $num$ and $pi$ are at addresses of **100** and **104**
  - Both are *assumed* to occupy **four bytes**
    - *Both* of these **sizes will differ**.
      - Depending on the **system configuration**.

---

- Various Points to remember

1. The contents of **pi** should **eventually** be *assigned* to the *address* of **integer variable**
2. These variables has **not been initialized** and therefore **contain garbage**.
3. There is **nothing inherent** to a **pointers implementation** that *suggests* the actual **type of data** that it may be referencing or whether the contents are valid.
4. The **pointer type** has been **specified** and compiler will **complain** on it **not being used correctly**

>---
>
>Garbage - The memory allocation can contain **any value**
>
>When **memory** that is **allocated** is *not cleared* > previous contents may be **anything**
>
>If the previous contents contain **a floating point number** > interpreting as integer may **not be useful** 
>
>Even with integer  > may not be correct
>
>Thus overall its always garbage.
>
>---

- A pointer can be used with **NO INIT**
  - It may not **always work** until its **properly INIT**

### How to read declaration

- READ BACKWARDS.

```c
const int *pci;
```

- Reading declaration backward **enables** to **progressively** understand the **declaration**

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210415224732818.png)

- Draw pictures in **longer complex pointer expressions**

### Address of Operator

- The Address of operator $\&$ $\to$ return the **operand address**
  - Can init pi pointer with **the address** of *num* using this **operator**.

```c
num = 0;
pi = &num;
```

- The variable **num** is *set to **zero***
- Pi set to **point** to the **memory location** where *num* stores this value.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210415225104225.png)

- Could have set pi too address of num when the variables were **declared**

```c
int num;
int *pi = &num;
```

- With this **declaration** $\to$ the **following statement** results in a **syntax error** on *most compilers*;

```c
num = 0;
pi = num;
```

- This error:

>```
>error: invalid conversion from 'int' to 'int*'
>```

- Pi is type **int pointer** and num is **type integer**
  - The *error* is saying you can not **convert** some **integer** to a **pointer** to the **data type integer**
    - Assignments of integers to a pointer will generally cause warning   / error.

- Pointers $\neq$ integers.
  - They may **both** be storing using the **same number of bytes** often
    - But are **not the same**
      - You can however **cast**

```c
pi = (int *) num;
```

- Not generate a **syntax error**
  - Program may **terminate** abnormally as the program **attempts** to *dereference* the **value** at *address **zero***.
- An address of **zero** $\to$ is *not always valid* for use in a **program** on **most operating systems**

---

>Good practice to **initialize a pointer** as *soon as possible*, as illustrated here
>
>```c
>int num;
>int *pi;
>pi = &num;
>```

### Displaying pointer values

- Variable address determined by following:

```c
int num = 0;
int *pi = &num;

printf("Address of num: %d  Value: %d\n",&num, num);
printf("Address of pi: %d  Value: %d\n",&pi, pi);
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210415233724206.png)

- The **printf** has other **field specifiers** that are useful when displaying **pointer values**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210415233913905.png)

- Use shown here

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210415234342713.png)

- %p $\to$ specifier differs from %x as it display **hex** in uppercase.
  - use %p specifier unless otherwise indicated.
    - Displaying pointer values consistently on **different platforms** is **hard**
      - An approach $\to$ is **cast pointer** as a **pointer to void** and display using %p format specifier

```c
printf("Value of pi: %p\n", (void*)pi);
```

#### Virtual memory and pointers

- The **pointer addresses** displayed on **virtual OS** $\to$ are often not *real physical memory address*
  - Virtual OS $\to$ enables the program to be **split** across the *machines physical address space*
- Application $\to$ split to **pages / frames**
  - These pages are **areas of main memory**
    - The pages $\to$ are *allocated* to **different** and possibly **non-contiguous** areas of memory and **may not be all in memory at the same time**
- If OS $\to$ needs memory **held by page** $\to$ may be swapped to **secondary storage**
  - Then **reloaded** at a **later time**.
    - Frequently at a **different memory location**.
- Provide virtual OS the ability to provider a **virtual operating system** with *considerable flexibility* in **memory management**

---

- Each program $\to$ assume entire access of **physical space** $\to$ false
  - the **address** used by some program is a **virtual address**
    - The OS will map this to a **real physical memory address** when *required*.

---

- Therefore the **code / data** in a *page* may be **different physical locations** as the *program executes*
  - The virtual address of **app** does **not change**
    - They are what we see when we **examine** the *contents of a pointer*. 
- The **virtual addresses** are *transparently mapped* to the **real addresses** by the **operating system**
  - OS handles all of this $\to$ not something the **programmer** has *any control over* or **needs to worry about**
    - Understanding this issues explains the **addresses** returned by a **program** running in **virtual OS system**

---

### Dereferencing  a Pointer using the indirection operator

- The indirection operator $\to$ $*$ 
  - Returns the **value pointed** to by a **pointer variable**
    - Called **dereferencing**.

```c
int num = 5;
int *pi = &num;

printf("%p\n", *pi); // Display 5
```

- Use the **result** of *dereference* as the actual **l value**
  - The term of **l value** refers to the **operand** on the **left hand side** of the **assignment operator**
    - Every **l value** must be **modifiable** as they are being **assigned** a **value**.

- pi is the pointer to num therefore setting the deferenced pi to some integer say 200 will add the value 100 to the memory location of num.

```c
*pi = 200;
printf("%d\n" ,num) // display 200
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210513151317842.png)

### Pointers to functions

- A pointer can be **declared** to *point* a **function**
  - The *declaration* **notation** is **bit cryptic**
- Declaration of a **pointer** is **function**
  - The **function** is **passed** *void* and returns **void**
    - The pointers name is **foo**.

```c
void (*foo)();
```

- A **pointer** to a *function* is a **rich topic area**

### The concept of NULL

- There are various uses of the word null in c.

1. Null concept
2. Null pointer constant
3. Null MACRO
4. The ASCII NUL
5. A null string
6. The null statement

- When **NULL** is *assigned* to a **pointer**
  - It means the pointer does **not point to anything**.

- *Null concept* $\to$ the idea that a **pointer** can hold some **special value** that is **not equal** to **some other pointer**.
  - Does **not point to any area of memory**.
- **NULL pointers** will always be **equal to one another**
  - There can be a **null pointer type** for *every pointer type*, int,char etc.

---

- The **Null concept** is an *abstraction* by the **null pointer constant**
  - This constant may/may be a **constant zero**
    - A C programmer is **not concerned** in the **underlying representations**.

---

- The **NULL** *macro* is a **constant integer zero** *cast* to **pointer to void**
  - In **many libraries** $\to$ defined as such:

```c
#define NULL ((void *) 0)
```

- This is what is often **thought as** a **void pointer**
  - This definition is **found within many header files** such as *stdio.h*
- if some **non zero** *bit pattern* is used **by the compiler** to *represent* **NULL**
  - It is a **compilers responsibility** to ensure all uses of NULL or 0 in a **pointer** are treated as **null pointers**.
- The **internal representation** of *null* is **implementation defined**
  - The use of **NULL** and **0** are *language level symbols* that **represents** a **null pointer**

---

- The ASCII NUL $\to$ defined as **byte containing** containing **all zeros**
  - This is **not the same** as the **null pointer**.
- A *string* in $C$ is a **sequence of characters** terminated by **some zero value**.
  - The null string is **an empty string** and has **no characters**

---

- The **null statement** is a statement with **single semicolon**.

```c
pi = NULL;
```

> a **NULL POINTER** and an **uninitialized pointer are very different**
>
> An *unitialized* pointer can contain **any value** , however a NULL does **not reference any location in memory**

- Can assign 0 to a pointer
  - Cannot assign any other integer value
- Consider these **assignment operations**:

```c
pi = 0;
pi = NULL;
pi = 100; // Syntax error
pi = num; // Syntax error
```

- A **pointer** can be used as the **sole operand** of a *logical expression*
  - Test to see whether its **null** or not.

```c
if (pi){
    // Not NULL
} else {
    // Is NULL
}
```

> Either of **two following expressions** are valid however are **redundant** as you can just pass it in to if without direct check against NULL

```c
if (pi == NULL);
if (pi != NULL);
```

### Null / Not NULL

- Using NULL or 0
  - Either.
    - Some developers prefer NULL as its a **reminder** of **working with pointers**
      - Other feel this is required as **zero is hidden**. 
- NULL $\to$ should **not be used** in contexts **other than pointers**
  - may work but **not intended**. 
    - It can be **problem** when used in place of **ASCII** NUL character
      - This character is defined in any standard C header file
        - Its **equivalent** to the **character literal** '\0' $\to$ evaluates to the decimal 0.

---

- The meaning of zero changes depending 
  - may mean the integer zero / null pointer in various seperate contexts

```c
int num;
int *pi =0; // Zero refers to the null pointer, NULL
pi = &num;
*pi = 0; // Zero refers to the integer
```

- We are **accustomed operators**
  - The zero happens to **also be overloaded however**
    - This seems odd as we are not used **to overloading operands**

### Pointer to void

- A void pointer is for **generality** and can hold **references** to **any data type**
  - Examples to void is **shown below**

```c
void *pv;
```

- There are 2 main properties of this pointer
  1. A **void pointer** has the **same representation** and **memory alignment** as a **pointer** to **char**.
  2. A **void pointer** will **never equal** to **another pointer**
     - Two **void pointers** with both **NULL** are **equal**.

- Any pointer can be assigned with **void**
  - Can be **cast back** to the **original pointer type**
    - When this happens $\to$ the **value** becomes **equal** to the **original pointer value**
- This is shown in the following sequence below
  - A pointer to int becomes assigned to void pointer then back to int

```c
int num;
int *pi = &num;
printf("Value of pi: %pi\n", pi); // 100

void *pv = pi;
pi = (int *) pv;
printf("Value of pi: %p\n", pi); // 100
```

- Void pointers are used as **data pointers** and **not function pointers**
  - In **Polymorphism** in C section we will discuss this further.

> When using void pointers $\to$ if you cast **some arbitrary pointer to a vp**
>
> There is no preventing you from actually casting it some other pointer type.

- The **sizeof operator** can be **used** alongside a pointer to **void**
  - We can **not** use the **operator** with **void** as such **shown below**

```c
size_t size = sizeof(void *); // legal
size_t size = sizeof(void); // Illegal
```

- Use the **data type** of *size_t* for **sizes only** 

### Global and static pointers

- If some pointer is **static / Global** its value is **set to NULL** initially
  - Example of **global and static pointer**

```c
int *globalpi;

void foo() {
    static int *staticpi;
}

int main(){
    
}
```

- The memory layout of this

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210513172843935.png)

- *Stack frames* are **pushed** onto the stack
  - The **heap** is used for **dynamic memory allocation**.
    - The region above the **heap** is used for **static / global variables**
      - The diagram is **conceptual**.

- Static / Global variables are often placed in **data segment** that is *seperate* from the **stack and heaps one**

### Pointer size and types

- The **size of** pointers is an issue when we are **concerned** on **portability** and **compatibility** 
  - On **most modern platforms** $\to$ the *size of a pointer* is **normally the same** with no concern for the actual **data type**.
    - A **char pointer** has the **same size** as a **struct pointer**.

- Size of **function pointer** could be *different* to a **data pointer**.
  - Size depends on **machine in use / compiler**
- Windows is 32/64 bit which is the size of **lowest addressable memory space**.

### Memory models

- Introduction of 64 bit computing made the variety of sizes **more apparent** when memory is **allocated** for **data types**
  - With **different machines** and *compilers* come **different options** for *allocating* space to **C primitive data** types
    - The **common notation** used to *describe* the variety of **different data models** is *summarised* as below.

```c
I ,In, L, Ln, LL, LLn, P, Pn
```

- Every **capital letter** corresponds to **some integer , long or pointer**
  - The *lowercase* letters are the **number of bits** allocated for the **data type**
    - Summary of the models below where the size is the **number of bits**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210513174202240.png)

- Model depends on the **OS** and **compiler**
  - More than one model can be supported on the **same operating system**.
    - This is controlled **via compiler options**.

### Predefined Pointer-Related Types

- There are 4 predefined types
  - These are frequently used when working with **pointers** 
    - Include:

1. **SIZE_T**
   - Provide **safe data type** for **sizes**
2. **ptrdiff_t**
   - Created to **handle** *pointer arithmetic*
3. **intptr_t** and **unitptr_t**
   - Used for **storing pointer addresses**

---

#### Understanding size_t

- The type of *size_t* is the **maximum size** that any object can be within C
  - Its an **unsigned** *integer* as **negative numbers** do **not make sense**
- The purpose is to **provide portable means** of declaring a **size** that is **consistent** with the *addressable area of memory* on some system
  - The size_t type is used as **return type** for the **sizeof operator** and as the **argument** to a **variety of functions**
    - Such as **malloc** and **strlen**

> Good practice to use size_t when **declaring variables** for **sizes** such as the **number of characters** and **array indexes**
>
> Should be used for **loop counters**, **indexing into arrays** , **pointer arithmetic sometimes**.

- Declaration of **size_t** is **implementation specific**
  - Found in **one or more standard headers** 
    - Such as *stdio.h* and *stdlib.h* , defined as **follows**: 

```c
#ifndef __SIZE_T
#define __SIZE_T
typedef unsigned int size_t;
#endif
```

- Define directives ensure only **defined once**
  - The *actual size* is **dependent** on **implementation**
- 32 BIT system > 32 bits in length
- 64 BIT system > 64 bits in length
  - Normally **maximum possible** for **size_t** is **SIZE_MAX**

>Size_t can be used to **store pointer**, its not a good idea in **assuming** size_t is **same size as a pointer**
>
>Using size of operator with pointers, **intptr_t** is a better choice.

- Careful when printing **defined** as *size_t* 
  - These are **unsigned values** $\to$ if you chose **wrong format specifier** obtain **unreliable results**.
- The **recommended** format specifier **%zu** 
  - Alternative is %u / %lu as this is not available always.

---

- Consider the **following example** $\to$ define some variable as **size_t** and then **display** using **two different specifiers**

```c
size_t sizet = -5;
printf("%d\n", sizet);
printf("%zu\n", sizet);
```

- Since a **variable** of *type* ***size_t*** is for use with **positive integers**
  - Use of negative values presents problems on assigning negative number and %d then %zu:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210514111151015.png)

- %d interprets size_t as **signed integer**, where as %zu is for unsigned therefore becomes undefined.

```c
sizet = 5;

printf("%d\n", sizet); // Displays 5
printf("%zu\n", sizet); // Displays 5
```

- As **size_t** is **unsigned** $\to$ always **assign** some **positive number** to a *variable* of that type.

#### Using sizeof operator with pointers

- The **sizeof** operator $\to$ used to **determine** the *size of a pointer*
  - Following displays **the size** of a **pointer to char**:

```c
printf("Size of char* :%d\n", sizeof(char*)); // 4, 8 on my 64 bit system however.
```

> Always use the **sizeof** operator when **the size** of a **pointer** is **needed**

- The **sizeof** some **function pointer** 
  - Its consistent for a **given OS** and **compiler** *combination* 
    - Many compilers support the creation of **32 bit / 64 bit application** $\to$ possible that the same **program** *compiled* with **different options** use **different pointer sizes**.

- On **Harvard** architecture $\to$ the *code / data* is stored in **different physical memory**
  - Example $\to$ the intel MCS-51 (8051) microcontroller is **a Harvard machine**.
    - Intel no long manufactures this $\to$ there are **many compatible** derivatives available in use today
- The **small device** $C$ compiler **SDCC** supports this **type of processor**
  - Pointers on **this machine** can **range** from **1 to 4 bytes** in **length**
    - Therefore the **size of a pointer** should be **determined** when **need** as the size is **not consistent** in this **type of environment**.

---

#### Using intptr_t and uintptr_t

- The types of **intrptr_t** and **uintptr_t** store **pointer addresses**.
  - Provides **portable** / **safe** method of **declaring pointers**
    - Will be **same size** as *underlying pointer* used on the system.
- Used for **converting pointers** to the **integer representation** often.

---

- The **type uintrptr_t** is just **unsigned** version of **intptr_t**
  - Most operations on **intptr_t** is **preferred** 
    - The type of **uintptr_t** is **not as flexible** as **intptr_t** 
      - Illustration how to **intptr_t**

```c
int num;
intptr_t *pi = &num;

// Attempting to assign the address of an integer to a pointer of type uintptr_t , obtains an error

uintptr_t *pu = &num; // invalid conversion from int* to uintptr_t * (aka unsigned int*)

// Performing the assignment using a cast will work
intptr_t *pi = &num;
uintptr_t *pu =  (uintptr_t*) &num;

// Cannot use uintptr_t with other data types without the cast required.

char c;
uintptr_t *pc = (uintptr_t *) &c;
```

- These are used when **portability** and **safety** is an **issue**
  - Not use in the examples to **simplify the explanations**

> **WARNING** - AVOID CASTING POINTERS TO INTEGERS, FOR 64 BIT INTEGERS , INFORMATION WILL BE LOST IF THE INTEGER WAS ONLY FOUR BYTES

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210514115611095.png)

### Pointer operators

- There are **several operators** for using with **pointers**
  - So far > above is **dereferencing** and **addressing**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210514115755191.png)

### Pointer Arithmetic

- There are many arithmetic operations that can be **performed** on **pointers to data**
  - These include:

1. Adding an **integer** to a **pointer**
2. Subtracting an **integer** from a **pointer**
3. Subtracting **two pointers** from **each other**
4. Comparing **pointers**

- These operations are **not always** permitted on **pointers to functions**

#### Adding an integer to a pointer

- This is **common and useful**
  - Adding an **integer** to a **pointer** $\to$ the *ammount added* is the **product** of the **integer** times the **number of bytes** of the **underlying data type.**
- The **size of ** primitive data types **varies** from system > system
  - Table below $\to$ shows the **common sizes** found **in most systems**
    -  Unless **noted otherwise** $\to$ these values are used in this book.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210514120550788.png)

- use array of **integers** to show effects of addition
  - Each time is added to pi $\to$ **four added** to the **address** 
    - The **memory allocated** for these variables is **illustrated** below

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210514120957795.png)

- Pointers declared with **data types** so that these **arithmetic operations** are **possible**
  - Knowledge of the **data type size** enables **auto adjustment** of the **pointer values** in a **portable fashion**

```c
int vector[] = {28, 41, 7};
int *pi = vector; //pi : 100

printf("%d\n", *pi); // displays 28

pi += 1; // pi = 104

printf("%d\n", *pi); // displays 41

//.....
```

> When an **array name** is used **by itself** $\to$ returns the **address** of an **array**
>
> - This is the **address** of the **first element** of the **array**.

- For the **following sequence** $\to$ add **three** to the **pointer**
  - Variable pi contain the address 112 of pi

```c
pi = vector;
pi += 3;
```

- The **pointer** is **pointing to itself**
  - This is **not very useful** however illustrates the **need** to be **careful** with **pointer arithmetic**.

- Access of **memory** past the **end of an array** is **dangerous** , must be **avoided** 
  - There is **no guarantee** that **memory access** is some **valid variable**. 
    - Its easy to **compute** some **invalid / useless address**

- The following declarations **used to illustrate** the **addition** operation performed **with short** then a **char data type**

```c
short s;
short *ps = &s;
char c;
char *pc = &c;
```

- Lets **assume memory** is allocated **as shown**
  - The *addresses* here used **here** are **all on** a **four byte word** *boundary* 
    - The **real addresses** may **be aligned** on *different boundaries

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210515135306271.png)

- The **following sequence** will add **one to each pointer** $\to$ then display the **contents**.

```c
   printf("Content of ps before: %d\n",ps);
   ps = ps + 1;
   printf("Content of ps after:  %d\n",ps);

   printf("Content of pc before: %d\n",pc);
   pc = pc + 1;
   printf("Content of pc after:  %d\n",pc)
       
/* Content of ps before: 120
Content of ps after:  122
Content of pc before: 128
Address of pc after:  129 */
```

- The **ps pointer** is *incremented* by **two** as the size of a **short** is **two bytes**
  - The **pc pointer** is incremented **by its size of one byte** therefore incremented by 1.
    - These addresses **many not contain useful information**

#### Pointers to void and addition

- Most **compilers** enable **arithmetic** to be **performed** on a **void pointer** as *extension*
  - Assume the **size of a pointer** to **void** is **four**.
    - Trying to **add one** to some **pointer** that is **void** in a **syntax error**.
- Declare and attempt to **add one to the pointer**

```c
int num = 5;
void *pv = &num;
printf("%p\n",pv);
pv = pv+1;        //Syntax warning
```

- This warning will be **produced**

  > warning: pointer of type 'void *' used in arithmetic [-Wpointerarith]

- this is actually wrong in this , its incremented by 1 byte rather than 4 bytes

#### Subtracting an integer from a pointer

- Integer values can **be subtracted** from *pointers* in **same way** as they are **added**
  - The **size of** the *data type* **times** the *integer increment* value is **subtracted** from *the address*
    - To show the **effects** of *subtracting* an *integer* from **a pointer** $\to$ use an **array of integers**.
- Memory created for **these variables** is illustrated below

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210515142337852.png)

```c
int vector[] = {28, 41, 7};
int *pi = vector + 2;  // pi: 108
    
printf("%d\n",*pi);    // Displays 7
pi--;                  // pi: 104
printf("%d\n",*pi);    // Displays 41
pi--;                  // pi: 100
printf("%d\n",*pi);    // Displays 28
```

- Each **time that one** is *subtracted* from **pi** $\to$ **four is subtracted**.

#### Subtracting two pointers

- When a **pointer** is *subtracted* from **another**
  - We obtain the **difference** of the **addresses**
    - This difference is **not normally** very useful **except** for **determining** for the **order** of **elements in an array**.

```c
int vector[] = {28, 41, 7};
int *p0 = vector;
int *p1 = vector+1;
int *p2 = vector+2;
    
printf("p2-p0:  %d\n",p2-p0);    // p2-p0:  2, find difference of last element in first element is 2, index seperate by 2.
printf("p2-p1:  %d\n",p2-p1);    // p2-p1:  1
printf("p0-p1:  %d\n",p0-p1);    // p0-p1:  -1, indicates it precedes the element directly.
```

- Illustrates how memory is **allocated** for **this example**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210515145359495.png)

- The type of **ptrdiff_t** is the **portable method** of which to **express** the *difference* between **two pointers**
  - For the **previous example** $\to$ the **result** of *subtracting* the **two pointers** is *returned* as a **ptrdiff_t** type.
    - Since **pointer sizes** can *differ* $\to$ this **type** simplifies the **task** of **working** with **their differences**.

- Do **not confuse** this *technique* $\to$ using **the dereference operator** to **subtract two numbers**
  - For this **example** $\to$ we use **pointers** to *determine the difference* between the **value store** in the *arrays'* first and **second elements**:

```c
printf("*p0-*p1:  %d\n",*p0-*p1);  //  *p0-*p1:  -13
```

### Comparing Pointers

- Compared **using the standard** *comparison operators*
  - Comparing **pointers** is *often not very useful*
- When **comparing pointers** to *elements of* an **array** $\to$ the **comparisons results** can be used to determine , 
  - the **relative ordering** of *the arrays element* 

- Use the **vector example** that was in the section above **of subtracting two pointers**
  - Several comparison **operators** are *applied to the pointers*
    - Their **results** are **displayed** as $1$ for **true** and **0** for **false**.

```c
int vector[] = {28, 41, 7};
int *p0 = vector;
int *p1 = vector+1;
int *p2 = vector+2;
    
printf("p2>p0:  %d\n",p2>p0);    // p2>p0:  1
printf("p2<p0:  %d\n",p2<p0);    // p2<p0:  0
printf("p0>p1:  %d\n",p0>p1);    // p0>p1:  0
```

### Common uses of pointers

- Pointers used **in variety of ways**
  - In this **section** we *examine* different **ways of using pointers**:

#### Multiple levels of indirection

- Pointers use **different levels** of **indirection**.
  - It is **not uncommon** to *see a variable declared* as **pointer**, to a **pointer** $\to$ called **double pointer**

- A good example $\to$ is when **program arguments** are *passed* to **the main function**
  - Using the **traditionally** named *argc* and *argv* parameters.

- Example here uses three arrays.

```c
char *titles[] = {"A tale of Two cities", "Wuthering Heights", "Don Quixote", "Odyssey", "Moby-Dick", "Hamlet", "Gullivers travel"};
```

- There are **two additional arrays**
  1. Maintain list of **best books**.
  2. Maintain list of **English books**.

- Alongside holding **copies of the titles** $\to$ they hold the **address** of a **title** within the **titles array**
  - Both arrays will **need to declared** as a **pointer** to a **pointer** to **a char**
    - The **arrays elements** hold the **addresses** of the **titles** *arrays element* $\to$ this **will avoid hanging to duplicate memory** for *each title* and results in **single location** for **titles**.
      - If a **title needs** to **be changed** $\to$ then the **change** will **only have** to be **performed in one location**

- The **two array** have been **declared below**
  - Each array element **contains** a **pointer** that *points* to the **second pointer** to **char**.

```c
char **bestBooks[3];
char **englishBooks[4];
```

- The **two arrays** *initialised* and **one of their elements** is **displayed**
  - Shown below $\to$ in the **assignment statements** $\to$ the **value** of **righthand side** is *calculated* by **applying** the **subscripts** first
    - Followed by the **address of operator**
- For **example** $\to$ the **second statement** $\to$ assigns the **address** of the **fourth element** of **titles** to **second element** of **best books**

```c
    bestBooks[0] = &titles[0]; // stores the referece to the first item of the titles list which is a char pointer in itself.
    bestBooks[1] = &titles[3];
    bestBooks[2] = &titles[5];
    
    englishBooks[0] = &titles[0];
    englishBooks[1] = &titles[1];
    englishBooks[2] = &titles[5];
    englishBooks[3] = &titles[6];
    
    printf("%s\n",*englishBooks[1]);   // Wuthering Heights
```

- Memory **is allocated** for **this example**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210515161843534.png)

- Multiple levels of **indirection** provides **additionally** *flexibility* in **how code** can be *written and used*.
  - **Certain operations** would **otherwise** be *more difficult*.
    - If the **address** of some **title changes** $\to$ require **modification** only to the **the title array**
      - Would **not have to modify** the **other arrays**

- There is **not an inherent** *limit* on how many **levels of indirection** are **possible**.
  - Using **too many levels** of *indirection* can be **confusing** and **hard to maintain**

### Constants and Pointers

- Using **const keyword** with *pointers* is a **rich / powerful** aspect of $C$. 
  - Provides **various protections** for many **problem sets**.
- A **pointer** to a **constant** is very useful.

#### Pointers to a constant

- Pointer **defined** to **point** to some **constant** 
  - This means **the pointer cannot** be used to **modify** the **value** that its **referencing**.
- An **integer** and an **integer constant** are **declared**
  - A **pointer** to an **integer** and a **pointer to integer constant** are declared then **initialised** to the **respective integers**

```c
int num = 5;
const int limit = 500;
int *pi; // Pointer to an integer
const int *pci; // Pointer to a constant integer

pi = &num;
pci = &limit;
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210516140243355.png)

```c
   printf("  num - Address: %p  value: %d\n",&num, num);
   printf("limit - Address: %p  value: %d\n",&limit, limit);
   printf("   pi - Address: %p  value: %p\n",&pi, pi);
   printf("  pci - Address: %p  value: %p\n",&pci, pci);
```

- When executed $\to$ the sequence **produce values** that are **similar** to **following**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210516140325620.png)

- *Dereferencing* a **constant pointer** is *fine* 
  - If we are **simply reading** the **integer value** $\to$ reading is **legitimate** and **necessary capability of course**.

```c
printf("%d\n", *pci);
```

- **Cannot deference** a *constant pointer* to **change** what the **pointer references**
  - We can **change the pointer** however $\to$ the **pointer is value** is **not constant**.
    - Pointer can be **changed** to **reference** some other constant integer or **simple integer**.
- The **declaration** just **limits** the **ability** to *modify* the *referenced variable* through the **pointer**

```c
pci  = &num; // this is legal essentialy as we are not changing the item being references we are just changing the actual reference itself.

*pci = 200; // syntax error 'pci': cannot assign to a variable that is const
```

- The **pointer** behaves as **pointing** to any  **constant integer**, the pointer itself is not constant in what it points to
  - A **constant pointer** visualised as shown in **figure 1-12** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210516141520733.png)

- The **clear boxes** represent **variables** that *can be changed*
  - The **shaded boxes** represent **variables** that **cant be changed**
    - The shaded box pointed to by pci **cannot be changed** using **pci** 
      - The **dashed line** indicates that the **pointer** can **reference that data type**
- In the **previous example** the **pci** pointed to **limit**.

---

- The **declaration** of **pci** as a **pointer** to some **constant integer** means:
  1. pci **can be assigned** to point to **different constant integers**.
  2. pci can be **assigned** to point to **different non constant integers**
  3. pci can be **deferenced** for **reading purposes**.
  4. pci can **not** be **dereferenced** to **change what it points to.**

> The **order of the type** and the **const keyword** is **not important**, the **equivalent below**
>
> ```c
> const int *pci; // pointer to constant integer
> int const *pci; // integer pointer that is constant
> ```

#### Constant pointers to **non constants**

- Can **declare a constant pointer** to a **non constant**
  - While the **pointer cant be changed**.
    - The data **pointed to by** is **modifiable**.



```c
int num;
int *const cpi = &num;
```

- From this **declaration**:
  1. CPI must be **initialized** to a **non constant variable**
  2. CPI **cannot be modified** 
  3. The data **pointed** to by CPI can **be modified**.

- **Conceptually** $\to$ this *type of pointer* can be **visualised**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210516143135907.png)

- Its possible to **dereference cpi** and *assign* a **new value** to whatever **cpi is referencing**
  - The following are **two valid assignments**.

```c
*cpi = limit;
*cpi = 25;
```

- Attempting to **initialize cpi** to the **constant limit** as **shown below** $\to$ obtain a **warning**.

```c
const int limit = 500;
int *const cpi = &limit;
```

- Warning appears as **follows**:

```c
warning: initialization discards qualifers from pointer target type
```

- If **cpi** reference the **constant limit**.
  - The **constant** may be **remain constant**.
- Once some **address** has been **assigned to cpi** $\to$ can **not assign** some **new value** to **cpi** as shown below:

```c
int num;
int age;
int *const cpi = &num;
cpi = &age;
```

- Error message generated is **shown below**

> 'cpi' : you cannot assign to some variable that is const

#### Constant pointers to constants

- A **constant pointer** to a **constant** an *infrequently* used **pointer type**
  - The pointer **cant be changed** and the **data it points too**  can **not be changed** through the pointer
    - Example of a **constant pointer** to **constant integer** is:

```c
const int * const cpci = &limit;
```

- A constant pointer to constant can be **visualised** as **shown below**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210516144607201.png)

- As with **pointers to constants**
  - Its **not necessary** to *assign* the **address** of a **constant** to *cpci*
    - Instead we **could** have used **num** as **shown below**.

```c
int num;
const int *const cpci = &num;
```

- When the **pointer is declared** $\to$ must **initialize it**
  - If we **dont initialize** as **shown below** $\to$ obtain the **syntax error**.

```c
const int *const cpci;
```

- The **syntax error** will be **similar** to *following*:

> 'cpci': const object must be initialized if not extern

- Given **constant pointer** to a **constant we cannot**
  1. **Modify** the **pointer**
  2. **Modify** the *data* **pointed** to by the **pointer**

- Attempt to **assign** a **new address** to **cpci** result in **syntax error**

```c
cpci = &num;
```

- Syntax error follows:

> 'cpci': cannot assign to a variable that is const

- Attempting to **dereference** the **pointer** and **assign a value** as *shown below*
  - We get a syntax error:

```c
*cpci = 25;
```

- The **error generated** is similar to **the following**

> 'cpci' : cannot assign to a variable that is const expression must be a modifiable lvalue

- *Constant pointers to constants are rare*

#### Pointer to (constant pointer to constant)

- Pointers to **constants** can have **multiple levels of indirections**
  - Declare a **pointer** to the **cpci** pointer **explained** in the **previous section**
    - Reading **complex declarations** from *right to left* helps **clarify** theses **types of declarations**.

```c
const int * const cpci = &limit;
const int * const *pcpci;
```

- A **pointer** to a **constant pointer** to some **constant** , visualised as so.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210516150506083.png)

- The **following illustrates** the use
  - The **output** of this **sequence** should **display** *500 twice*.

```c
printf("%d\n", *cpci);
pcpci = &cpci;
printf("%d\n", **pcpci);
```

- Following table **summarises ** the **first four types** of **pointers** that are **discussed** in the **previous section**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210516150816378.png)

#### 