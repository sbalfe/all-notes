# Compiling and linking

> RANDOM NOTES
>
> - Functions are not always defined in headers for the simple idea that a change would cause recompilation of various source files which have this header as a dependency (**changes to header means the recompilation of many files**)

- C++ programs are **source files** and **headers**
  - These *text files* but **need not be**
    - For *simplicity*  we will **speak** as if they are **text files**.

---

- Much of text in C++ and **header files** represents **declarations**
  - The *declarations* establish existence of **==entities==** such as 
    1. Functions
    2. Namespaces
    3. Objects
    4. Templates
    5. Types
    6. Values
- C++ has **no specific rules** about *which declarations* must go into **source files** and **which must go into headers**.
  - There are **many conventions**

---

- For some function 
  1. *declare* it in a header, and...
  2. *define* it in a **corresponding source file**
- For some *function* that **inline**, *constexpr* or *consteval* then:
  - Define it in a **header file** in its **entirety**.

- This exists as for **best practices reason** in terms of *reliability* / maintainability and efficiency.

---

- Special cases that **dont fit the general guidelines**

- Mishandling of these leads to

  1. Build (compile / linker) errors.
  2. non portable or erroneous runtime behaviours.
  3. speed or space inefficiencies.

- Rely of compilation and linking experience to deal with this.

---

## Declarations and definitions

- **Declaration** $\to$ program statement that says to the compiler
  1. Name and some attributes for an entity
  2. May be here / may be somewhere else.
- **Definition** $\to$ says:
  1. Here is a name and the complete set of attributes for an entity that's right here.

- All definitions are declarations.
- Not all declarations are definitions.

---

```c++
int foo(int n); // non defining declaration
```

- no body
- tells compiler how to **generate code that calls this function**
- doesn't tell compiler how to generate code for the **function itself**

---

```c++
int foo(int n){ // definition
    return n + 5;
}
```

- Brace enclosed body
- Tells the compiler everything it needs to know to **create the function**.

---

- C++ $\to$ an **object declaration** (outside of class) is a **definition** unless it contains an **extern specifier** and **no initializer**

```c++
int i; // definition
extern int j; // non-defining declaration
extern int k = 42 // definition
```

- An **object definition** $\to$ allocates the **storage** for the object.
- An object declaration does not do this.

---

- A **non defining declaration** say it *exists* but its *not here*.

- **Definition** $\to$ exists and here it is.
- Similar distinction for classes and templates.

---

## Build Modes

- Release in either
  1. Release build $\to$ program as intended for **distribution**
  2. Debug build $\to$ program augmented with symbolic information to facilitate debugging.
- Focus on Release build

## Translation process

- Traditional toolchains $\to$ C++ programs are built in **three steps** (*translated*)
  1. Pre-processor.
  2. Analysis and code generation.
  3. Linking.

- Compiling encompasses this entire process (building is the better term)
- Also separate into **compiling** (preprocess, analysis , code generation) and **linking**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220822195559130.png)

- Linker takes compiled cpp files in form of object files to create the executable.

### Pre-processing

- Each source file is pre-processed separately.
- Each source file is pre-processed seperately.
- Pre-processor $\to$ performs text transformations
  1. Header inclusion
  2. Conditional compilation
  3. Macro substitution

- Output of pre-processor is a **translation unit**
  - Concept that the standard uses for *descriptive purposes*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220822200348249.png)

- Pre-processor need not generate the **translation unit as a file**, very rare.

### Analysis and code generation

- Compiler analyses each translation unit.
- The compiled output is called an **object file** or **object module** (no relation to C++20 Modules)
- Stored in a **file system** $\to$ object files often use `.o` / `.oo` / `.obj` extension.

>inside object file:
>
>1. Data $\to$ 0/1 that become part of the exe program
>2. Metadata $\to$ information the linker uses to combine object files into the executable program

- Data:
  1. Machine instructions (generated program code, more or less)
  2. Values (initialized program objects, statically initialized)
- Metadata $\to$ symbols and values for
  1. Function and objects and their associated addresses
     - Some names represent **defs** (*definitions*)
       - This file **knows what this function does**
     - Some **refs** (*non defining declarations*).
       - This file must ask another file where is the definition.
  2. Program section names and the associated contents
     1. text / code for machine instructions
     2. *literal* for **initialised read only data**
     3. *data* for initialised **read write data**
     4. *bss* for **uninitialized read write data** $\to$ space for static variables that are initialized somewhere else.

https://stackoverflow.com/questions/3045603/what-does-an-object-file-contain - what an object file contains

> Only mostly executable
>
> - Machine instructions in an **object file** are *only mostly executable*
> - Instruction can contain **==external references==** to an *entity* defined in **another translation unit**
> - It uses a *placeholder* for the entity in the object file
> - the **linker** makes the instruction **executable** by *replacing* the *placeholder* with the **entity's address**.
> - Additionally $\to$ machine instructions in an object file are often generated to be **relocatable**.
>   - uses relative offsets such as move 4 from this address to locate this item.

### Linking

- Linker **resolves** any **external references** by *combining*
  1. Object files
  2. Libraries of previously compiled object files.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220822205309088.png)

- Handles *relocation* of values and code too.

## The One Definition Rule

- Every entity a program **uses** must be **defined** in that program

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220822205553057.png)

- Linker matches **each external reference** with the **corresponding definition**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220822205824344.png)

- external reference defined in another object file is located by the linker and used as the definition.
- If there are no definitions found then it becomes an **undefined reference**.

---

- Linker can find **more than one matching definition**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220822210156851.png)

- Returns a **multiple defined error**.

---

- Both examples violate ODR
- In generated exe $\to$ every entity has **a single definition**
- The **translation process arrives** at that **one definition** differently for **different kinds of entities**.

---

- Some entities **must be defined in exactly ONE translation unit**
  1. non inline variables
  2. non inline, non template functions
     - Constexpr / consteval functions are **implicitly inline** $\to$ Like other inline functions, they are **not included here**.

- This is just a **part of the ODR**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220822210535029.png)

### Header Entities and the ODR

- These entities are allowed to have **more than one definition**
  - Each definition appears in **a different translation unit**.
  - All definitions are **identical** (same sequence of tokens)
- There are **additional requirements** $\to$ can usually meet them all by
  - Defining the entity in a **single header** that can be *included by itself* (no dependency on inclusion of other headers).

- If the **definition meets these requirements** $\to$ program behaves as if there were a **single definition** for the **entire program**.

- Otherwise their is **undefined behaviour**.
  - Object file not being recompiled after a header file changes when its included.
- Hard to verify therefore the standard does **not have tools to do so**.

### Linking header entities

- Standard has no specification on how the **compiler / linker** must resolve symbols for entities that are defined in header files
- code generator $\to$ records **definitions** and **non defining declarations** as *metadata in object files*.

- Solution is to **change the metadata** for the **entities** that are often defined in header files to let the **linker identify** and **remove duplicate definitions** (template instantiation).

---

- Visual C++ marks these entities with a **pick any flag**
- Notifies the **linker**
  1. May see multiple definitions for some **entity**.
  2. Choose one **discarding the rest**.

- Referred to as **==weak symbols==**
  - Toolchains use the term weak symbols, there are some differences in **behaviour among them**.

## Templates

- We define templates **in header files**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220822212629593.png)

### Explicit template specialization

- Treat these like **non inline entities**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220822213409956.png)

- Function templates are **not functions**
  - Its just a **recipe** for **generation of functions**.

- Class template is not a class
  - Recipe for **generating a class**

- Same idea for **other template entities** such as **variable templates.**
- Holds for **both primary templates** and **partially specialized templates**.

---

- An explicit specialization has the definition already has been generated and therefore is a single class or function
- Accessed over the **template syntax**, treated like a **non template class**.
- Therefore has the header and source file separation.

### Explicit template instantiation

- Linker decides where an **entity generated** from some **template is defined**
  - Example $\to$ keeping the **definition** in *one object file* then **discarding the rest**.
- If you must control where some **generated entity** is *defined* $\to$ use **explicit template instantiation**
  - This is *another* feature that affects **what goes where**.

---

- template declaration for code generation purposes.
- Other files now assume existence of template definition 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220823011809197.png)

- explicitly instantiated template is a **single entity**
  - Not a *recipe for generating entities*.
- Define in source file but the body still comes from the header from the primary template definition.
- Compiler generates a single definition for the entity in that **source files translation unit**
- Do the same for classes, generates for every class member regardless of whether its actually used.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220823013324005.png)

- 

## Memory Maps

- Code generators and linkers separate entities to **different program sections**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220822233623670.png)

- The **linker** is *often responsible* for *arranging* these **sections** in **memory**.
- This arrangement is the **program** *memory map*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220822234725892.png)

---

### Controlling memory maps

- linkers let you control how they generate the map via **linker scripts**
- Scripts can be **valuable** when *working on platforms* with **multiple memory spaces**
- Embedded systems have RAM / FLASH memory
- Using linker script $\to$ an **embedded developer** can *load entities* into RAM and others into flash

----



![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220823005739055.png)

- Works fine for **entities** defined in the source file.
  - Non inline, non template function
  - non inline varibales
  - explicitly specialized templates.
- Does **not work well** for templates that are *instantiated* in *multiple object files*
  - Linker **eliminates** the *duplicate definitions* from **all** but *one* of the **object files**
  - Does not tell which one it keeps.
- You can use explicit template instantiation to **choose** which *object file* contains a **template definition**.

---

## Summary

### Non inline function

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220823013532934.png)

- a.cpp `add` is an **internal reference** to its own codes definition in the same translation unit.
- `entites.h` has the **declaration**
- `b.cpp` has an **external reference** to the **definition** in `a.cpp`. Resolved by the linker.

---

- inline function that is **not actually inlined**
  - no need to refer to code in other places, code is right there inline
- Inline functions are **recursive**
  - Recursive functions are **hard to inline** if the **arguments** are **not known at compile time**.
  - Arguments here are **run time values**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220823013911026.png)

- Two definitions that are the same but the linker eliminates one.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220823014038752.png)
