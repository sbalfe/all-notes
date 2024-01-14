# Modern C++ Course Github

### 4 - Entities and Control flow ==COMPLETE== 

- [x] Struct, Bitfield and Union

  - **Struct**

    ```cpp
    // A struct (structure) aggregates different variables into a single unit
    struct A {
        int x;
        char y;
    };
    
    // It is possible to declare one or more variables after the definition of a struct
    struct A {
        int x;
    } a, b;
    
    // Enumerators can be declared within a struct without a name
    struct A {
        enum {X, Y};
    };
    
    A::X; // Accessing the enumerator X of struct A
    
    ```

  - **Bitfield** 

    ```cpp
    // Bitfield
    // A bitfield is a variable of a structure with a predefined bit width. A bitfield can hold bits instead of bytes.
    
    struct S1 {
        int b1 : 10;   // range [0, 1023]
        int b2 : 10;   // range [0, 1023]
        int b3 : 8;    // range [0, 255]
    }; // sizeof(S1): 4 bytes
    
    struct S2 {
        int b1 : 10;
        int    : 0;    // reset: force the next field to start at bit 32
        int b2 : 10;   
    }; // sizeof(S2): 8 bytes
    
    ```

  - **Union** 

    - Special data type that allows us to store different data types in the same memory location

    - `union` is only as big as neccessary to hold its *largest* data member

    - `union` is a kind of overlapping storage

      <img src="./images/image-20231128143850704.png" alt="image-20231128143850704" style="zoom:67%;" />

    ```cpp
    // A union allows to store different data types in the same memory location.
    union A {
        int x;
        char y;
    }; // sizeof(A): 4 bytes
    
    A a;
    a.x = 1023; // bits: 00..00001111111111
    a.y = 0;    // bits: 00..00001100000000
    cout << a.x; // print 512 + 256 = 768
    
    // NOTE: Little-Endian encoding maps the bytes of a value in memory in the reverse order. y maps to the last byte of x
    
    // Contrary to struct, C++ allows anonymous unions (i.e. without a name)
    // C++17 introduces std::variant to represent a type-safe union
    
    // Anonymous union example
    union {
        int integer;
        float realValue;
    };
    
    integer = 1024;
    cout << realValue; // This will output the float representation of the binary content of integer
    
    // std::variant example from C++17
    #include <variant>
    #include <iostream>
    
    std::variant<int, float> data;
    data = 10; // data now holds an integer
    data = 10.5f; // now data holds a float
    
    // std::get to safely access the values
    try {
        std::cout << std::get<float>(data) << std::endl;
    } catch (const std::bad_variant_access&) {
        std::cout << "Wrong variant access" << std::endl;
    }
    ```

### 5 - Memory Management ==COMPLETE== 

- [ ] Heap and Stack

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

- [ ] Initialisation

  - 

- [ ] Constants, Literals, const, constexpr, consteval and constinit

  - **Constant** - is an expressiong that is evaluated at compile time

  - **Literal** - is a fixed value that can be assigned to a constant. Literals are the tokens of a C++ program that represent constant values embedded into the source code.
  
    <img src="./images/image-20231128142944006.png" alt="image-20231128142944006" style="zoom:80%;" />
  
  - **const** - keyword that indicates objects never changing value after their initilisation (must be initialised when declared)
  
    - Evaluated at compile time if the right expression is also evaluated at compile time
  
  - **constexpr** - specifier declares that the expressions can be evaluated at compile time
  
    - const guarantees the value of a variable to be fixed overall the execution of the program
  
    - constexpr implies const
  
    - constexpr helps performance and memory usage
  
    - constexpr can impact compilation time maybe
  
      ```cpp
      const int v1 = 3;               // compile-time evaluation
      const int v2 = v1 * 2;          // compile-time evaluation
      
      int a = 3;                      // "a" is dynamic
      const int v3 = a;               // run-time evaluation!!
      
      constexpr int c1 = v1;          // ok
      // constexpr int c2 = v3;       // compile error, "v3" is dynamic
      
      ```
  
  - **constexpr function** - Guarantees compile-time evaluation of a function as long as all its arguments are evaluated at compile time
  
    ![image-20231128144317526](./images/image-20231128144317526.png)
  
    
  
  - **Limitiations of constexpr**
  
    <img src="./images/image-20231128144341877.png" alt="image-20231128144341877" style="zoom:67%;" />
  
    
  
  - `constexpr` non-static member functions of run time objects cannot be used even if all constraints are respected
  
  - `static constexpr` member functions dont present this issue as they dont depend on any instance
  
  - **consteval** - or immediate functions, guarantees compile-time evaluation of a function. A non-constant value always produces a compilation error
  
    ```cpp
    consteval int square(int value){
        return value * value;
    }
    
    square(4); // compile-time evaluation
    int v = 4; // v is dynamic
    //square (v); // compile error.
    ```
  
  - **constinit** - guarantees compile time initialisation of a variable. A non cosntant always produces a compilation error
  
    - Value of a variable can change during the execution
  
    - `const constinit` does not imply `constexpr`, while the opposite is true.
  
    - `constepxr` requires compile time evaluation during its entire lifeteime
  
      ```cpp
      constexpr int square (int value){
          return value * valule;
      }
      
      constinit int v1 = square(4); // compile time evaluation
      v1 = 3; // v1 can change 
      int a = 4; // v is dynamic
      // constinit int v2 square(a) // compile error as a is dynamic
      ```
  
  - **std::is_constant_evaluated** utility is to evaluate if the current function is evaluated at compile time
  
    ```cpp
    #include <type_traits> // std::is_constant_evaluated
    
    constexpr int f(int n) {
        if (std::is_constant_evaluated())
            return 0;
        return 4;
    }
    
    int x = f(3); // x = 0
    
    int v = 3;
    int y = f(v); // y = 4
    
    ```

### 8 - Polymorphism and Operator Overloading

- [ ]  Polymorphism

  - **Virtual Table** - Each class in the hierarchy has a pointer to a virtual table where it resolves the fuction call, as the pointers are different a liskov subsituted class can be used to acces an overriden function. If you call a function that is in the parent class but not overrided in the child class the pointer to the virtual table resolves this back up to the parent class as it contains a pointer to that. essentially the pointers chain together to link to the correct function call. Override will just switch the function pointer to point to that class instead.

    ![image-20231128133438562](./images/image-20231128133438562.png)

    ![image-20231128133500026](./images/image-20231128133500026.png)

    

- [ ] Inheritance Casting and Run-time Type identification

  - **RTTI** - Available for polymorphic classes that have at least one virtual method
    - `dynamic_cast` - conversion of polymorphic types
    - `typeid` - identifying the exact type of an object
    - `type_info` - type information returned by the `tyepid` operator.

  - **Class casting** allows implicit or explicit conversion of a class into another one across its hierarchy

    ![image-20231128133905521](./images/image-20231128133905521.png)

    - **Upcasting** $\to$ Conversion between a derived class reference or pointer to a base class
      - Can be implicit or explicit
      - Safe
      - static cast or dynamic cast
    - **Downcasting** $\to$ Conversion between a base class reference or pointer to a derived class
      - It is only explicit
      - can be dangerous
      - static cast or dynamic cast
    - **Sidecasting** $\to$ Conversion between a class reference or pointer to an other class of the same hiearchy level
      - It is only explicit 
      - Can be dangerous 
      - dynamic cast.

- [ ] C++ Object Layout

  <img src="./images/image-20231128134959630.png" alt="image-20231128134959630" style="zoom: 67%;" />

  - **Aggregate** - An aggregate is a type which supports aggregate initialisation (form of list initliation) through curly braces syntax {}

    ![image-20231128134331284](./images/image-20231128134331284.png)

    ```cpp
    struct Agg1 {
        int x; 
        struct Agg2 {
            int a;
            int b[3];
        } y;
    }
    
    int main(){
        int array1[]
    }
    ```

  - **Trivial Class** - A class that is trivially copyable (supports memcpy)

    ![image-20231128134608687](./images/image-20231128134608687.png)

  - **Standard Layout Class** is a class with the same memory layout of the equivalent struct or union (useful when communicating with other languages)

    ![image-20231128134652794](./images/image-20231128134652794.png)

    ```cpp
    struct StandardLayout1 {
        StandardLayout1(); // user-provided constructor
        int x;
        void f(); // non-virtual function
    };
    
    class StandardLayout2 : StandardLayout1 {
        int x, y; // both are private
        StandardLayout1 y; // can have members of base type
                           // if they are not the first
    };
    
    struct StandardLayout3 { }; // empty
    
    struct StandardLayout4 : StandardLayout2, StandardLayout3 {
        // can use multiple inheritance as long only
        // one class in the hierarchy has non-static data members
    };
    ```

  - **POD (Plain Old Data)** 

    ![image-20231128134928027](./images/image-20231128134928027.png)


### 9 / 10  - Template metaprogramming ==READ ALL NOW== 

### 12 - Translation units ==READ ALL PT2== 

### 13 - Code Conventions ==READ ALL NOW==

### 14 / 15 - Ecosystem ==READ NOT URGNET== 

### 16 - Utilities

- read the whole thing

### 17 - Containers, iterators and algorithms ==READ ALL NOW==

- read the whole thing

### 19 - Advanced Topics

- [ ] UB
- [ ] Error Handling

### 20 / 21/ 23 - Optimisation ==IMPORTANT FOR LATER RESEARCH== 

