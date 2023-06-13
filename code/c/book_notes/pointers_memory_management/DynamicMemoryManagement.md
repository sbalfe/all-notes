# Dynamic Memory Management in C

- Pointer power $\to$ ability **to track** the **dynamically allocated memory**.
  - The **management** of **this memory** via **pointers** forms the **basis** for variety of **operations**
    - Including manipulation of **complex data structures**.

---

- C program executes with a **runtime system**
  - This is **often the environment** provided by an **operating system** 
    - The **runtime** system **supports** the *stack / heap* along with **other program behaviour**.
- Memory **management** is important for all programs
  - Memory may be managed **by runtime system implicitly** 
    - Such as **when memory** is **allocated** for **automatic variables**.
      - For this case $\to$ **variables** are **allocated** to the **enclosing** *functions stack frame*.
- For **static / global** 
  - The memory is placed in **applications data segment** $\to$ it becomes **zeroed out**
    - This is **seperate** area from **executable code** and **other data** *managed* by **runtime system**.

---

- The **ability** to *allocate* then **deallocate memory** enables some **application** to **manage memory** much *more efficiently*
  - Alongside **greater flexibility** 
    - Rather than having **to allocate memory** to *accommodate* the **largest possible size** for some **data structure**
      - Only the **actual amount** required **needs to be allocated**.

---

- Arrays **are fixed size** in C before C99
  - If we require the ability to hold **a variable** *number of elements*.
    - Forced to **declare** array large enough to hold **maximum** number of **employees** that we expect to need.
- If you **underestimate** the size $\to$ **forced** to **recompile** with **larger size** or **take other approaches.**
- If we **overestimate** the size $\to$ **waste space**

---

- Ability to **dynamically allocate memory** $\to$ helps dealing with **data structures** using a **variable number of elements**
  - Such as **linked** list **or queue**.

> C99 introduced Variable Length Arrays - Determined at runtime and **not compile** , once created , arrays

- C support **dynamic memory management** where objects are **allocated memory** from **heap**
  - This is done via **functions** to *allocate* and *deallocate memory*
    - The **process** is *referred* to as **dynamic memory management**.
- We use malloc / realloc / free/ NULL

- **Dangling pointers** are a *common problem*
  - We **present examples** to *illustrate* when **dangling pointers** occur along with **techniques** to **handle the problem**.

---

## Dynamic Memory Allocation

- Steps for **basic** *dynamic memory allocation* in **C**:

1. Use a **malloc** type function to **allocate memory**
2. Use this to **support** the **application**
3. **Deallocate** the memory using **free function**

---

- May vary $\to$ **most common**
  - Below $\to$ **allocate memory** for an **integer** using the *malloc function*.
- The **Pointer** assigns **five** to the **allocated memory**. 
  - The memory is then released using **free function**.

---

```c
int *pi = (int *) malloc(sizeof(int));
*pi = 5;
printf("*pi: %d\n", *pi);
free(pi);
```

- When **the sequence executed**.
  - Displays the **number 5**.
- Illustrates **how memory** is *allocated* **before** the **free** is executed
  - Assume example code is found in **main** function unless **noted otherwise**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210516160202498.png)

- The **malloc** function **singular arg** $\to$ specifies the **number** of **bytes** to allocate
  - If **successful** $\to$ returns a **pointer** to the **memory allocated** from the *heap*
    - If **fail** $\to$ returns **null pointer**.
- **sizeof** operator enables **portability** and **determines** the **correct** the *number of bytes* for the **host running**
  - Example $\to$ attempt to **allocate** memory for **some integer**, Assume size is **4**

```c
int *pi = (int *) malloc(4);
```

- Can **not assume** that the number of bytes required is 4.
  - Depends **on memory model**.
    - sizeof therefore makes it portable $\to$ return **correct size** regardless of **where** the **programs executing**.

> Common Error with **dereference** operator shown below
>
> ```c
> int  *pi;
> *pi = (int *) malloc(sizeof(int));
> ```
> Problem on **LHS** $\to$ we **dereferencing** the pointer $\to$ **assigns** the *address* returned by **malloc** to the **address** in **pi**
>
> - If this is **first time** as an **assignment** to **made** to the **pointer** $\to$ the **address** is often **invalid**
>
> Correct: 
>
> ```c
> pi = (int *) malloc(sizeof(int));
> ```
>
> - Dereference operator should **never** be **used in this situation**.

- The **free** function > in conjuction with malloc to **free memory**.

> Each time the **malloc** function $\to$ **similar function**
>
> - is called $\to$ a corresponding to **call** to **free function** must be **made** when **the application** is *done* with **the memory** to *avoid memory leaks*

- Once **memory** has **been freed** $\to$ *not* be **accessed again** 
  - Normally $\to$ **not intentionally access** it *after deallocation*
    - This **may occur** *accidentally* as seen in **dangling pointers**
- System behaves **in an implementation dependent manner** 
  - Always **NULL** to a **freed pointer**

---

- When **memory** is **allocated** $\to$ additional information is **stored** as *part of a data structures*
  - maintained by **heap manager**.
- This **information** includes *amongst other things*:
  1. **Block size**
  2. **Place adjacent** to the **allocated block**
- If **app** writes **out of block of memory** $\to$ the **data structures** can **be corrupted**.
  - Leads to **strange program** $\to$ *behaviour* or even **heap corruption**

---

- Consider the **following code sequence**.
  - Memory **allocated for a string** $\to$ enabling up to **five characters** alongside **the null terminator** NUL.
- The **for loop** $\to$ writes *zeros* to **each location** but *does not stop* after **writing six bytes**
  - The **for statements terminal** condition *requires* that it **write eight bytes**.
    - The *zeros* being **written** are **binary zeros** and not **the ASCII** value for the **character zero**.

```c
char *pc = (char*) malloc(6);

for(int i=0; i<8; i++) {
   	*pc[i] = 0;
}
```

- **Extra memory** has been *allocated* at the **end** of the **six byte string**
  - This is **extra memory** used by **the heap manager** to *keep track* of **all memory allocation**

- Writing past the **end of the string**
  - the **extra memory** will *be corrupted*
    - The **extra memory** $\to$ shown **following** the *string* in **this example**
      - The **actual placement** and **original content** actually **depend** on the **compiler**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210516184425360.png)

### Memory leaks

- A **memory leak** occurs when **allocated memory** is *never used again* but is **not freed**, this *can happen when*
  1. The **memory address** is **lost**
  2. The **free** function is *never invoke* even **though it should be**
     - This is **called** some **hidden leak**.
- Problem with **memory leaks**
  - The memory **cant be reclaimed** and **used later** $\to$ the **amount of memory** that is *available* to the **heap manager** is *decreased*
    - If memory is **repeatedly** allocated **then lost** $\to$ the **program** can **terminate** if *more memory* is required however **malloc** can **not allocated**, as there is **no memory** 
- In **extreme case** $\to$ ENTIRE OS CRASH

---

- Shown :

```c
char *chunk;
while (1) {
    chunk = (char *) malloc(1000000);
    printf("Allocating\n");
}
```

- The *variable chunk* $\to$ is **assigned memory** of the **heap**.
  - However **this memory isn't free** before **some other block** of is **assigned to it**.
    - The **application** will **run out of memory** then **terminate abnormally**
      - At minimum $\to$ **minimum not being used efficiently**.

#### Losing the address

- An *example* of **losing the address** of **memory** is *illustrated* in the **following code** sequence where **pi** is *reassigned to a new address*
  - The **address** of **first allocation** of **memory** is *lost* when **pi** is **allocated memory** a **second time**.

```c
int *pi = (int *) malloc(sizeof(int));
*pi = 5;
...
pi = (int *) malloc(sizeof(int));
```

- This is shown in **figure 2-3**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210518120110534.png)

- The **before** and **after images** refer to the **programs state** *before / after* the **second mallocs execution**
  - The *memory at address* **500** has *not been released*.
    - The program **no longer holds** this **address anywhere**.

- Another **example** *allocates* **memory** for a **string**
  - Then **initializes** it $\to$ then **displays** the **string character** by **character**.

```c
char *name = (char *) malloc(strlen("Susan")+1);
strcpy(name, "Susan");
while (*name != 0){
    printf("%c", *name);
    name++;
}
```

- This leaves the **name pointer** directed at **null** after the **loop iteration**
  - At the **end** $\to$ the **name** is *left pointing* to the **strings** NUL termination character, shown below
    - The **allocated memory's** *starting address* has **been lost**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210518121153693.png)

#### Hidden memory leaks

- Memory **leaks** can *occur* when **the program** should **actually release** *memory* does *not*.
  - A **hidden memory leak occurs** when an **object** is **no longer needed**.
    - Result of **programmer oversight**.

- Problem with **this type of leak** $\to$ the **object** is **using memory** that's **no longer needed** and **should be returned to heap**.
  - In **worst case** $\to$ the **heap manager** may *not be able to allocate* **memory** *when requested*.
    - Possibly making the **program terminate** $\to$ holding **unneeded memory** at **best case**.
- *Memory leaks* $\to$ **can also occur** when *freeing structures* using the **struct keyword**
  - If **the structure** *contains* **pointer** to *dynamically allocated memory* $\to$ then **these pointers** may required to **be freed** *before* the structure is **freed**.

---

### Dynamic memory allocation functions

- There are **several allocation functions** for *dynamic memory*
  - What is available could be **system dependent** $\to$ the *following functions* are *found* on **most systems** in the **stdlib.h** *header file*
    1. MALLOC
    2. REALLOC
    3. CALLOC
    4. FREE

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210518123127602.png)

- **Dynamic memory** is *allocated* **from the heap**
  - With **successive memory** *allocation calls* $\to$ there is **no guarantee** *regarding the order* of **memory** or *the continuity memory allocated*
    - **However** *allocated* **will be** *aligned according* to the **pointers data type**.
- For **example** $\to$ a **four byte integer** would be allocated on an **address** *boundary* that is **evenly divisible by four**.
  - The **address returned** by the **heap manager** will *contain* the **lowest bytes address**

---

- Figure below $\to$ **malloc** function *allocates* **four bytes** at **address 500**.
  - The **second use** of **malloc**  function **allocates** *memory* at **address 600**.
    - They **both on four byte address boundaries** $\to$ they *did not allocate* **memory** from **consecutive memory location**.

### Using The **Malloc Function**

- The **function** *malloc* allocates a **block** of **memory from the heap**
  - The **number** of **bytes** is *allocated* a **specified** by some **single argument**.
    - Its **return type** is a **pointer to void** , if memory is **not available** then **NULL is returned** 

- The **function** does **not clear** or *otherwise modify the memory*
  - The **contents** of **memory** should be **treated** as if **contained garbage**
- The prototype:

```c
void * malloc(size_t);
```

- This has one argument of **size_t**.
  - Careful when **passing** variables **to this function**
- Massive issues when there **are negative numbers**
  - Some systems $\to$ a **NULL value** is *returned* when **the argument is negative**
- If **malloc** is used with **an argument of zero**
  - The **behaviour** is **implementation specific** $\to$ may **return a pointer** to NULL or a **pointer** to some **region** of **zero bytes** *allocated*
- If **malloc** function used with **NULL** argument 
  - Normally generate **warning** and **execute** returning the **zero bytes**.

- Typical use:

```c
int *pi = (int *) malloc(sizeof(int));
```

- Following steps **are performed** when *malloc* function **is executed**

1. Memory **allocated** from **the heap**.
2. Memory is **not modified** or *otherwise cleared*
3. First **byte's** address is **returned**.

> As malloc may **return** a **NULL** value if **unable** to *allocate memory*, good practice to **check for NULL** before using the **pointer** as **follows**.
>
> ```c
> int *pi = (int*) malloc(sizeof(int));
> 
> if(pi != NULL) {
>     // Pointer should be good
> } else {
>    // Bad pointer
> }
> ```

#### Cast / not cast

- Before **pointer to void** was added to C
  - *Explicit* casts were **required** with **malloc** to **stop generation** of **warnings** when *assignments* were *made* **between incompatible pointer types**
    - As a **void pointer** can be **signed to** *ANY OTHER POINTER TYPE* $\to$ the **explicit casting** is **no longer required**.
      - As **developers** consider **explicit casts** a **good practice because**

- Some says its **good practice** to **cast**:

1. They **document** the **intention** of the **malloc function**
2. They **make** the **code** *compatible* with $C++$ 
   - Requires **explicit casts** 

- Using **casts** will **be a problem** if you **dont include** the **header file** for **malloc**
  - Compiler may **generate warnings**.
- Default C $\to$ assumes **functions** return an **integer**
  - Failing to **include** a **prototype** for **malloc** $\to$ will **complain** when you attempt to **assign an integer** to a **pointer**.

#### Failing to allocate memory

- Declaring point **with no memory allocated** to the specified address.
  - Often **contains garbage** $\to$ result in **invalid memory reference**.

```c
int *pi
    
printf("%d\n", *pi);
```

- Figure below:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210528134513315.png)

- Results in **runtime exception**
  - This is very **common in strings**.

```c
char *name;
printf("Enter a name: ");
scanf("%s",name);
```

- Can not read into unallocated memory therefore this would not work

#### Not using the right size for the malloc function

- **Malloc** $\to$ allocated the **specified number of bytes** as specified by the **arguments**.
  - Cautious when using **this to function** to **allocate the correct number of bytes**.
- 10 doubles require **80 bytes for example**

---

```c
double *pd = (double*) malloc(NUMBER_OF_DOUBLES * sizeof(double));
```

```c
const int NUMBER_OF_DOUBLES = 10;
double *pd = (double*)malloc(NUMBER_OF_DOUBLES);
```

- The code **only allocated 10 bytes**.

#### Determination of ammount of memory allocated

- There is **no standard method** to determine the **total memory allocated by the heap**.
  - Compilers **provide extensions** sometimes for **this purpose**.
- There is **no standard method** for **determining** the *size of some memory block allocated* by the **heap manager**.

---

- Allocate **64 bytes** $\to$ for **some string**
  -  The **heap manager** allocates **additional memory** to *manage this block*
    - The **total size allocated** and the **amount** used the **by heap manager** is the **sum of the two quantities**.

---

- **malloc executes** $\to$ it **allocates** the *amount of memory* requested **then return the memory's address**.
  - If **underlying OS** uses *lazy init* $\to$ where it **does not allocate** until **accessed**. 
    - A **problem** $\to$ if there is **not enough memory** actually *available* to **allocate**.
- Answer is **runtime / OS** *dependent*
  - This is **very unlikely** to even **occur** however.

---

#### Using malloc with static and global pointers

- Can **not use a function call** when *init* a *static/global variable*
  - In **following code** $\to$ declare **a static variable** then attempt to **initialize using malloc**.

```c
static int *pi = malloc(sizeof(int));
```

- Generates **compile time error message**
  - occurs with **global variables too**.
- Avoided with **static variables** by using a **seperate statement** to *allocate memory* to the **variable**.

```c
static int *pi;
pi = malloc(sizeof(int));
```

> From **compiler standpoint** there is a **difference** of the *init* operator $=$ 
>
> Alongside the **assignment operator** $=$. 

#### Using calloc function

- The *calloc function* $\to$ allocates and **clear memory** at the **same time**
  - The *prototype* is as follows:

```c
void * calloc(size_t numElements, size_t elementSize);
```

> Clearing memory is setting to **all binary zeros**

- Allocate dependent on **numElements** and **element size** *multiplied*.
- A **pointer** is *returned* to the **first byte of memory**.

- If the function **cant allocate memory** $\to$ NULL is **returned**
  - This was originally used for **allocation of memory** for **arrays**.

---

- If either **input** is **0** $\to$ a **null pointer** *MAY* be **returned.**
  - If **unable to allocate** $\to$ a **null pointer** returned and **the global variable** *errno* = ENOMEM (**out of memory**).
    - This is a **POSIX** error code and **not on all systems**.

- Example where **pi** is *allocated* a total of **20 bytes** that *all contain zeros*

```c
int *pi = calloc(5, sizeof(int));
```

- Achieve same result using **malloc** and **memset**

```c
int *pi = malloc(5 * sizeof(int));
memset(pi, 0, 5 * sizeof(int));
```

> memset fills a **block with some value**
>
> 1. First arg = **pointer** to the **buffer** to **fill**
> 2. Second arg = **value** used to **fill the bugger**
> 3. Last arg = number of **byte to set**

- Use **calloc** when **memory** must be **zeroed** out
  - The **execution time** may take **longer than malloc**.

> Function **cfree** is **no longer required** 
>
> - Early days of c $\to$ used to **free memory allocated by calloc.**

