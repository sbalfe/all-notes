# Operators

- Table shows **precedence and associativity** 
  - The **higher** in the **table** $\to$ the **higher precedence** of *execution* the **operator** has **within some expression**.
- Compiler uses this order in multi operator expressions and associativity if **equal**.



### Precedence

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210522134043956.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210522134139533.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210522134151338.png)

https://stackoverflow.com/questions/5209602/pointer-arithmetic-ptr-or-ptr

- Function calls have **higher precedence** than + 

```cpp
int a = b + c + d;
int a = b() + c() + d();
```

- Means the **three functions** are called in b/c/d order and **return values** are **summed** via the **left - right associativity**
  - This may be important as $d$ can **depend** on **global data** altered by **other two functions**.
- Makes code **more readable** and *easier to understand* if **you explicitly** specify the **precedence** by **grouping expressions** with *parentheses*  
- Built in operators are **overloaded** 
  - The **same syntax** used regardless of **which built in type** is used for **operands**
    - The **operands** must be the **same type** $\to$ if **different** $\to$ the **compiler** perform **default conversions**

- Other cases $\to$ **have to perform a cast** to **indicate explicitly** what you mean.

# Built in operators

## Arithmetic operators

- +/-/ '/ ' / * / % need **little explanation** other than the **division** and **modulus operator**.
  - All act on **integer and real** *numeric types* except for $\%$ $\to$ used with **integer types**.
- If you **mix the types** $\to$ such as **floating point and integer**.
  - compiler performs **automatic conversion**.
    - $/$ division performs **as expected** for **two floating point variables** $\to$ produces **result** of **division** of **two operands**.
- When **you perform** between **two integers a / b** the result **is whole number** of the **divisor** $b$ in the **dividend** $a$ 
  - The **remainder** of the *division* obtained by **modulus**. 
    - For any integer $b$ (other than zero) can say that **an integer** expressed as follows:

```cpp
(a / b) * b + (a % b)
```

- The **modulus** *operator* can **only be used with integers**
  - Want to **get remainder** of the **floating point division** $\to$ use **standard function** $std::remainder$ 

- Cautious in **integer division**
  - The **fractional parts are discarded**
    - if you require the **fractional parts** $\to$ must **explicitly** *convert* the numbers to **real numbers**

```cpp
int height = 480;
int width = 640;
float aspect_ratio = width/height;
```

Obtain aspect ratio of 1 when it should be 1.333

- To ensure floating point division is **performed** $\to$ rather than **integer division** $\to$ can **cast either / both** the *dividend* or **divisor** to a **floating point number**.

## Increment and Decrement operators

- Two versions:
  1. **prefix**
     - operator placed on **left.**
     - return **value** *after* the **operation**.
  2. **postfix**
     - operator placed on **right**.
     - return **value before** the **operation**.

- ++ $\to$ operator **will increment** *the operand* , -- $\to$ decrement

---

- So **the following code** $\to$ will **increment one variable** and *use it to assign* **another**

```cpp
a = ++b; // prefix
```

- The **prefix operator** used so the **variable $b$** is *incremented* and **variable** $a$ is **assigned** to the **value** after $b$ **has been incremented**.
  - Alternative way of **expression** this is:

```cpp
a = (b = b + 1);
```

- Postfix:

```cpp
a = b++; // postfix
```

- That *means* that **variable $b$** is **incremented**
  - Variable $a$ is **assigned** $b$ before its incremented.

- Alternative way of **expressing this is**:

```cpp
int t;
a = ( t = b, b = b +1, t);
```

- Statements **uses comma operator**
  - $a$ assigned to a **temporary variable $t$** in the **right most expression**.
- The **increment / decrement** operators **can be applied** to both **integer / floating point number**
  - Operators can be **applied to pointers** $\to$ when **special meaning**.
    - Incrementing a **pointer variable** $\to$ means to **increment the pointer** by **size of type** pointer **to by the operator**.

## Bitwise Operators

- Integers **can be regarded** as a *series* of **bits** of $0$ / $1$ 
  - **Bitwise operators** $\to$ act *upon* these **bits compared** to the **bit** in the **same position** in **other operand**.
- *Signed integers* use a **bit** to **indicate** the **sign**
  - **Bitwise operators** act upon **every bit** in some integer.
    - Often only sensible to **apply on unsigned integers**.
- For the **following** $\to$ all the **types** are marked as **unsigned**
  - Treated as **not having a sign bit**.

---

- $\&$ $\to$ bitwise AND $\to$ each **bit** in the **left hand operator** is compared with **bit** in the **righthand operand** in the **same position**

```cpp
    unsigned int a = 0x0a0a; // this is the binary 0000101000001010 
    unsigned int b = 0x00ff; // this is the binary 0000000000001111 
    unsigned int c = a & b;  // this is the binary 0000000000001010 
    std::cout << std::hex << std::showbase << c << std::endl;
```

- Using **bitwise** $\&$ with $\text{0x00ff}$ has the **same effect** as **providing** a *mask* that masks **out but lowest byte**

---

- Bitwise OR $\to$ | $\to$ return a **value** of $1$ if **either / both** bits **in the same position** are $1$ 
  - And a  value of $0$ only if **both are $0$**. 

```cpp
    unsigned int a = 0x0a0a; // this is the binary 0000101000001010 
    unsigned int b = 0x00ff; // this is the binary 0000000000001111 
    unsigned int c = a & b;  // this is the binary 0000101000001111 
    std::cout << std::hex << std::showbase << c << std::endl;
```

---



- A **use** of the **bitwise AND &** 
  - To **find** a **particular bit** or **specific collection of bits** is *set.*

```cpp
    unsigned int flags = 0x0a0a; // 0000101000001010 
    unsigned int test = 0x00ff;  // 0000000000001111 

    // 0000101000001111 is (flags & test) 
    if ((flags & test) == flags)  
    { 
        // code for when all the flags bits are set in test 
    } 
    if ((flags & test) != 0) 
    { 
        // code for when some or all the flag bits are set in test  
    }
```

- *flags* variable has our **required bits**
  - The **test variable** is a *value* we are **examining** 
- The value (flags / test) has only the bits in **test variables** that are **also set** within **flags**
  - If the **result** is *exactly* the **same** as the **flags variables** then every bit in **flags** are set **in test.**

- The **exclusive OR** operator ^ 
  -  test when the **bits are different** $\to$ resultant bit is $1$ if **the bits in the operands** are *different*
- Used to **flip specific bits**:

```cpp
    int value = 0xf1; 
    int flags = 0x02; 
    int result = value ^ flags; // 0xf3 
    std::cout << std::hex << result << std::endl;
```

- Final **bitwise operators** is the **bitwise complement ~ **
  - This **operator** is applied to **a single integer** operand and **returns** a **value** where *every bit* is the **complement** of the **corresponding** bit in the **operand**
    - operand 1 $\to$ the result is 0, if the bit in the operand is 0
      - The result is 1
- All bits are examined $\to$ must be aware of the **size of the integer**.

### Boolean Operators

- $==$ $\to$ test of **equality between operators** to see whether they are **exactly the same**
  - Two **real numbers may not** be the *same*. 

```c++
    double x = 1.000001 * 1000000000000; 
    double y = 1000001000000; 
    if (x == y) std::cout << "numbers are the same";
```

- The **double** is **floating point** held in **8 bytes**
  - This is **not precise enough** *used here* 
  - Value **stored** in $x$ *variable* is $100000009999999.9999$ (2 - 4 dp)

---

- $!=$ operator **tests** if **two values** are **not true**
- $<$ , $>$ $\to$ test to check which value is greater.

```cpp
    int x = 10; 
    int y = 11; 
    bool b = (x > y); 
    if (b) std::cout << "numbers same"; 
    else   std::cout << "numbers not same";
```

- There is a **greater precedence** of $=$ than $>=$ 
  - use parentheses to **prevent** it being **applied** first.

```c++
    int x = 10, y = 10, z = 9; 
    if ((x == y) || (y < z)) 
        std::cout << "one or both are true";
```

- Pointless second test as the first part of the expression is always true.
- Compiler provides code to perform this short circuit for you.

```cpp
    if ((x != 0) && (0.5 > 1/x))  
    { 
        // reciprocal is less than 0.5 
    }
```

- Code to **tests** to see if the **reciprocal** of $x > 0.5$ (conversely, that is $x$ is **greater than 2**)
  -  If $x$ variable has value of $0$ $\to$ then **the test** $\frac{1}{x}$ is **error but** for this case the **expression** is is *never executed* as the **left operand** to $\text{&&}$is **false** 

### Bitwise shift operators

- Shift the **bits** in the **left hand operand integer** by the **specified number** of **bits** given in the **right hand operand** with the **specified direction**
  - Shift **one bit left** $\to$ **multiplies** the *number by two*
  - Shift **one bit right** $\to$ **divides** the *number by two*

- In the **following** $\to$ some **2 byte integer** is **bit shifted**

```cpp
    unsigned short s1 = 0x0010; 
    unsigned short s2 = s1 << 8;  // left shift by 8 places = 2^8 multiplicaiton
    std::cout << std::hex << std::showbase; 
    std::cout << s2 << std::endl; 
    // 0x1000  
    s2 = s2 << 3; 
    std::cout << s2 << std::endl; 
    // 0x8000
```

- s1 variable has **fifth bit set** ($0\text{x}0010$ or **16**) 
  - s2 variable has **this value** shifted **by 8 bits** which is **multiplied by 256** .
- Then shifted again by 3 to the left and restored.   
---

- Operator **discards any bit** that **overflow**
  - Therefore if you have **top bit set** and **shift integer** one **bit left** that **top bit will be discarded**.

```cpp
s2 = s2 << 1; 
std::cout << s2 << std::endl; 
// 0
```

- Final **shift left by one** results in **value of 0**
  - Important to **that** when **used with stream**, the **operator** $<<$ means **insert** into **stream** 
    - Used with **integers** it means **bitwise shift**.

### Assignment operators

- The assignment operator $=$ assigns some **lvalue** (*a variable*) on **the left** with **the result** of the **rvalue** (*a variable or expression*)

```cpp
int x = 10; 
/* first line declares an integer initializes it to 10 */

x = x + 10; 
/* second line alters the variable by adding another 10, has a value of 20*/
/* this is the assignemnt operator*/
```

- The **assignment operator** allows you the **change the value** of **some variable** based on the **variables** *value* using an **abbreviated syntax**
  - Written as follows:

```cpp
int x = 10;
x += 10;
```

- An **increment operator** such as this $\to$ applied to **floating point types**
  - If **applied to a pointer** $\to$ the **operand** indicates how many **whole items addresses** the **pointer** is **changed by**.
    - If an **int** is **4 bytes** and you **add 10** to the **integer pointer** pointer is **incremented by 40** as this is the size of a single integer in memory.

---

- Can have $*=$ and $/=$ etc for most of the operators that operate in this kind of way.
  - All except for $\text{%=}$ can be used for **both floating point** types and integers
    - The **remainder assignment** can only **be used for integers**.

---

- $<<=$ left and right shift $>>=$ 
- Bitwise And **&=** and $|=$ and XOR **^=**      ^15ee8d
  - Only **makes** sense to **apply** these to **unsigned integers** 
    - Therefore **multiplying** by eight can be **carried out by both of these lines**.

---

```cpp
i *= 8;
i <<= 3
```

[[Operators#^15ee8d]]