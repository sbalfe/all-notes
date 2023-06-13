## Classes

https://stackoverflow.com/questions/2023977/difference-of-keywords-typename-and-class-in-templates

- Allows us to **create our own types**
  - Can have *operators* and can be **converted** to *other types*
- Can be used like **built in types** with the **behaviour** that *you define*
  - This feature called classes
- Advantage is that we **encapsulate** data in the **objects** of the **chosen type**
  - Use the **type** to *manage* the **lifetime** of that data
    - Can **define** the *actions* that can be **performed** on **that data**
- Other words $\to$ define **custom types** that have **state and behaviour**
  - basis of **OOP**

---

## Writing classes

- When you use built-in types, the data is directly available to whatever code has access to that data.
-  C++ provides a mechanism (const) to prevent write access, but any code can use const_cast to cast away const-ness. 
- Your data could be complex, such as a pointer to a file mapped into memory with the intention that your code will change a few bytes and then write the file back to disk. 
- Such **raw pointers** are **dangerous because other code** with access to the pointer could change part of the buffer that should not be changed. What is needed is a mechanism to encapsulate the data into a type that knows what bytes to change, and only allow that type to access the data. This is the basic idea behind classes.

---

## Reviewing structures

- A struct can **encapsulate data**
  - Allows you to **declare data members** that are *built in types, pointers or references*
    - Creating a **variable** from that **struct**, $\to$ create an **instance** of said **structure** called an **object**.
- Create **variables** that are **references** to *this object* or **pointers** that *point to the object.*
  - even pass the **object** by *value* to some function where the **compiler** makes the **copy of the object**
    - It will call **struct copy constructor**.

---

- With *struct* $\to$ *any code* can have **access** to an **instance** (even through a **pointer or reference** ) can access the members of the object (**can be changed though**)

  - Used as such $\to$ struct is thought of **aggregate types** containing the states.

---

- The **members** of an instance of a **struct** can be **initialized** by *accessing* them *directly* via **dot operator**
  - Or using **`->`** through a **pointer** to the **object** 
- Can **initialize** an *instance* of a *struct* with an **initializer list** (braces)
  - This is *restrictive* as init list must **match** the *data members* in the **struct**
- Pointers can be members of structs
  - Must **explicitly take action** to *release memory* pointed to by said pointer
    - If you dont $\to$ **memory leak**

---

- A **STRUCT** is *one of the class types* in **C++**
  - The other are **==union and class==**

- *Custom types* defined as **struct / class** can have **behaviours / state**
  - C++ can let you **define** some *special functions* to **control how instances** are *created , destroyed, copied and converted*
    - Furthermore $\to$ can **define operators** on a *struct or class*
      - So you can **use operators** on **instances** in a *similar* way to **using** the **operators** on **built in types**
- There is a **difference** between **struct and class** addressed later
  - WHEN A CLASS IS MENTIOEND often assume the same applies struct also.

---

## Defining classes

- A class is defined in a statement, and it will define its members in a block with multiple statements enclosed by braces {}. 
- As it's a statement, you have to place a semicolon after the last brace.
-  A class can be defined in a header file (as are many of the **C++ Standard Library** classes), but you have to take steps to ensure that such files are included only once in a source file. 
- There are, however, some rules about specific items in a class that must be defined in a source file, which will be covered later.

---

- If you **peruse the C++ Standard Library**, you will see that classes contain member functions and, in an attempt to put all the code for a class into a single header file, this makes the code difficult to read and difficult to understand. 
- This may be justifiable for a library file maintained by a legion of expert C++ programmers, but for your own projects readability should be a key design goal.
-  For this reason, a **C++ class** can be **declared in a C++ header file,** including its member functions, and the actual implementation of the functions can be placed in a source file. This makes the header files easier to maintain and more reusable.

---

## Defining class behaviour

- A **class** can define functions that can **only be called** through an **instance of the class**
  - Such a function is a **method**
    - An object has a **state** $\to$ given by **data members** defined by the **class** and **initialized** when the **object is created**
- The methods on an object $\to$ **define** the **behaviour** of the **object** usually acting **upon the state of the object**
  - When **designing** a *class* think of **methods** in such a way
    - They **describe** that the **object** *does something*

```c++
    class cartesian_vector 
    { 
    public: 
        double x; 
        double y; 
        // other methods 
        double get_magnitude() { return std::sqrt((x * x) + (y * y)); } 
    };
```

- Has **two data members**
  - Represent $\to$ the *direction* of a **two dimensional** vector solved in the **cartesian x and y directions**
- Public $\to$ means that **any member** defined after **this specifier** are *accessible* by **code** defined **outside** of the *class*
  - by **default** $\to$ all members of a class are **private** unless indicate otherwise
- Private $\to$ means that the **member** can *only be accessed* by **other members of the class**

> *This is the difference between a* struct *and a* class*: by default, members of a* struct *are* public *and by default, members of a* class *are* private*.*

- This class $\to$ has a method called **get_magnitude**
  - This returns the **length** of the **cartesian vector**
    - This **function** acts on **two data members** of the class and returns a value.
- This is a **type** of *accessor* method
  - Gives **access** to the **state of the object**
    - Such a **method** $\to$ is a **typical** on a **class**
      - There is **no requirement** that methods return values, like functions , can take parameters also
- get_magnitude method called as such

```c++
    cartesian_vector vec { 3.0, 4.0 }; 
    double len = vec.get_magnitude(); // returns 5.0
```

- Here a cartesian vector object is created on the **stack** and **list init syntax** used to init it to a **value representing** a vector of (3,4)
  - The **length** of this vector is **5**
    - Which is the **value** returned by **calling** *get_magnitude* on the object.

---

## Using this pointer

- The **methods** in a class have **special calling convention**
  - In visual C++ $\to$ called **__thiscall**
    - The reason is that **every method** in a **class** has a **hidden parameter** called *this*
      - This is a **pointer of the class** type to the **current instance**

```c++
    class cartesian_vector 
    { 
    public: 
        double x; 
        double y; 
        // other methods 
        double get_magnitude() 
        { 
             return std::sqrt((this->x * this->x) + (this->y * this->y)); 
        } 
    };
```

- The **get magnitude method** returns the **length** of the *cartesian_vector object*
  - The **members** of the **object** are accessed via **-> operator**
- Shown before $\to$ the **members** of the **class** accessed via **this pointer**
  - It does make it **explicit** that the **items** are *members of this class*
- Can define a **method** on the **cartesian_vector** type to allow you to **change its state**

```c++
    class cartesian_vector 
    { 
    public: 
        double x; 
        double y; 
        reset(double x, double y) { this->x = x; this->y = y; } 
        // other methods 
    };
```

- The **parameters** of the *reset* have the **same names** as the *data members* of the class
  - However via using **this pointer** the *compiler* does not view as **ambiguous**.

---

- Can **dereference** the **this pointer** with the *** operator** to get **access** to the **object**.
  - Useful when a **member function** must *return a reference* to the **current object** 
    - As **some operators** we see later
  - Do via returning ***this**
    - A **method** in a class can **also pass** the **this pointer** to an **external function**
      - Means that **its passing** the *current object* by **reference** through a **typed pointer.**

---

## Using scope resolution operator

- Can define a **method** as *inline* in the **class statement**
  - Can also **seperate** the *declaration and implementation*
    - So the **method** is **declared** in the **class statement** but *defined elsewhere*
- When defining a **method** *out* of the **class statement**
  - must provide a **method** with the **name** of the **type** using **scope resolution operator**
    - Example via the **cartesian_vector** example.

```c++
    class cartesian_vector 
    { 
    public: 
        double x; 
        double y; 
        // other methods 
        double magnitude(); 
    }; 

    double cartesian_vector::magnitude() 
    { 
        return sqrt((this->x * this->x) + (this->y * this->y)); 
    }
```

- This method **defined out** of the **class definition**
  - However $\to$ its still the **class method**
    - So it has a **this pointer** that can be **used to access** the *objects member*
- Typically $\to$ class will be **declared in a header file** with **prototypes** for *methods* and the **actual methods** 
  - Are *implemented* in a **seperate source file**
    - For this case $\to$ using **this pointer** to *access class members* (methods and data members) $\to$ makes its **obvious** when you **take a look at source file** that the functions are **methods of some class.**

## Defining class state

- Class can contain **built in types** / data members / custom types
  - Data members declared in the class (created when an instance of the **class is constructed**)
    - They can also be **pointers to objects** created in the **free store** or *references* to objects created elsewhere.
- If you **have a pointer** to an item created in the **free store**
  - You need to know whose **responsibility** it is to **deallocate memory** that it points to.
- If you have **a reference** / pointer $\to$ to an **object** created on the **stack frame** somewhere
  - Make sure that the **objects** of your class **do not live longer** than that **stack frame**.

---

- When you declare *data members* as **public**
  - means the **external code** can *read / write* to the **data members**
- Can choose that you would **prefer read only access**
  - Make them private and provide read **access through accessors**.

---

```c++
    class cartesian_vector 
    { 
        double x; 
        double y; 
    public: 
        double get_x() { return this->x; } 
        double get_y() { return this->y; } 
        // other methods 
    };
```

- make the **data members** $\to$ *private*
  - means you *cannot use* the **init list syntax** to **init an object**
    - Addressed later.
- May **decide** to use an **accessor** to *give* write *access* to **a data member** and use this to **check the value**.

```c++
    void cartesian_vector::set_x(double d) 
    { 
        if (d > -100 && d < 100) this->x = d; 
    }
```

- This is for a **type** where the **range of values** must be **between** (not including) -100 and 100.

## Creating objects

- Can **create objects** on the *stack* / *free store*
  - using previous example above:

```c++
    cartesian_vector vec { 10, 10 }; 
    cartesian_vector *pvec = new cartesian_vector { 5, 5 }; 
    // use pvec 
    delete pvec
```

- This is **==direct initialization==** of the *object* and *assumes* the data **members** of *cartesian_vector* are **public**
  - The **vec** *object* $\to$ is *created* on the **stack** and **initialized** with an **initializer**
    - In the **second line** $\to$ object is **created in free store** and *initialized* with **init list**
- The **object** on the *free store* must *be **freed*** at some stage
  - Carried out **via pointer deletion**
    - The *new operator* will **allocate enough memory** in the *free store* for the **data members** of the class and for **any infrastructure** the class requires.

---

- A **new feature** of C++11 is **to allow direct initialization** to *provide default values* in the class.

```c++
    class point 
    { 
    public: 
        int x = 0; 
        int y = 0; 
    };
```

- This **means** that if **you create** an *instance* of *point* without **any other initialization** values
  - It will be **initialized** so `x` and `y` are **both zero**
- If the **data member** is a **built in array**
  - Can *provide* **direct initialization** with an **initialization list** in the class.

```c++
    class car 
    { 
    public: 
        double tire_pressures[4] { 25.0, 25.0, 25.0, 25.0 }; 
    };
```

- C++ standard library containers can be **initialized** with an **initialize list** 
  - So in this class **tire_pressures** , instead of declaring the type to **be double[4]**
    - Could use **vector<double>** or **array<double,4>** and init in the **same way**.

---

## Construction of objects

- C++ allows you to **define** *special methods* to **perform init of the object**
  - Called **constructors** 
- C++11 there are **three such functions** generated **by default**
  - Can provide **own versions if so**
    1. **==DEFAULT CONSTRUCTOR==** - Used to create an **object** with **default value**
    2. **==COPY CONSTRUCTOR==** - Used to create a **new object** on the *value* of **an existing object.**
    3. **==MOVE CONSTRUCTOR==** - Used to **create new object** using the **data** that is **moved from existing object**
- Other related functions
  - ==Destructor== - Called to **clean up resources** used by some object.
  - ==Copy assignment== - This **copies** the **data** from **one existing object** into **another existing object**
  - ==Move assignment== - This **moves data** from *one existing object* 

---

- The **compile created versions** will *implicitly* be made **public**
  - May choose to **prevent copying** or **assigning** by *defining* your own and **making private**.

- Alternatively just **delete** using **=delete syntax**
  - Can *provide* your **own constructors** that take **any parameters** you decide that **you need** to **init a new object**.

---

- A **constructor** is a *member* function that has the **same name** as the **type**
  - Must **not return a value**
    - Therefore **cannot return value** if the **construction fails** $\to$ potentially receive a **partially constructed object**

- Must handle via **debugging** 

## Defining constructors

- The **default constructor** used when an **object** is *created* without **a value** and **hence** the *object* will have to **be initialized** with a **default value**
  - The **point** declared previously **could be implemented as such**:

```c++
    class point 
    { 
        double x; double y; 
    public: 
        point() { x = 0; y = 0; } 
    };
```

- This **explicitly initializes** the *item* to a **value of zero**
  - To **create an instance** with **default values**, include **no parentheses**

```c++
    point p;   // default constructor called
```

- Important to **note** that **because of this syntax**, easy to **write the following mistake**

```c++
  point p();  // compiles, but is a function prototype!
```

- This **will compiler** as the compiler thinks you are **providing a function prototype** as *forward declaration*.
  - Obtain an **error** when you **attempt** to use the **symbol** `p` as a **variable**
  - Can **also call** the **default** *constructor* using *initialize* **list syntax** with **empty braces**

```c++
    point p {};  // calls default constructor
```

- It **does not matter** for this case $\to$ where the **data members** are *built in types*
  - Initializing data **members** in the **body** of the **constructor** like this **involves** a *call* to the **assignment operator** of the *member type*
- More efficiently use **direct initialization** rather with a **member list**

```c++
 point(double x, double y) : x(x), y(y) {}
```

- The **identifiers** outside the **parentheses** are the *names* of the **class members** 
  - The **item inside** are *expression* used to **init** the *member* (for this case > a **constructor parameter**)
- This **example** uses *x* and *y* for the **parameter names**
  - Do **not have to do this**.
    - This is **only given** here as **illustration** that the *compiler* can **distinguish** between **parameters** and **data members**
- Can also use **brace initializer syntax** in the *member list of a **constructor**

```c++
  point(double x, double y) : x{x}, y{y} {}
```

- Can **call this constructor** when creating an object as such:

```;
point p(10.0, 10.0);
```

- Can create an **array of objects**:

```c++
point arr[4];
```

- Creates **four point objects** instantly
  - Can be accessed via **indexing arr array**
    - When creating an **array of objects** $\to$ the **default constructor** called on the items
      - Can **not call any other constructor** therefore must **init separately**.

---

- Can give **default values** for *constructor parameters*
  - In the **following code** $\to$ the **car class** has values for **four tires** (*first two* are for **the front tires**) and for spare tires.
- One constructor with **mandatory values** that is used for **front and back tires**
  - Along with **optional value** for the *spare*
    - If a **value** is *not provided* for the **spare tire pressure** $\to$ a **default value is used**.

```c++
    class car 
    { 
        array<double, 4> tire_pressures;; 
        double spare; 
    public: 
        car(double front, double back, double s = 25.0)  
          : tire_pressures{front, front, back, back}, spare{s} {} 
    };
```

- This **constructor** can be *called* with **either two values** or **three values**

```c++
    car commuter_car(25, 27); 
    car sports_car(26, 28, 28);
```

## Delegating constructors

- Constructor can **call another constructor** using the **same member list syntax**

```c++
    class car 
    { 
        // data members 
    public: 
        car(double front, double back, double s = 25.0)  
           : tire_pressures{front, front, back, back}, spare{s} {} 
        car(double all) : car(all, all) {} 
    };
```

- Here $\to$ constructor that takes one value **delegates** to the **constructor** that takes three parameter
  - in this case using the default values.

## Copy constructor

- A **copy constructor** used
  1.  **pass an object** by *value* 
  2.  **return by value**
  3. *explicitly* construct an **object** based on another object.

- The *last two lines* of the **following** $\to$ create a **point object** from *another point object* 
  - Both cases the **copy constructor called**

```c++
    point p1(10, 10); 
    point p2(p1); 
    point p3 = p1;
```

- The **final line** seems like it uses the **assignment operator**
  - it actually calls the **copy constructor**

- Copy constructor may be **implemented as such**

```c++
    class point 
    { 
        int x = 0;int y = 0; 
    public: 
        point(const point& rhs) : x(rhs.x), y(rhs.y) {} 
    };
```

- The **initialization access** is the *private data* members on **another object** (*rhs*)
  - This is **acceptable** as the *constructor parameter* $\to$ is the **same type** as the *object being created*
    - The **copy operation** may not be **this simple**
- Example $\to$ if the **class contains** some *data member* that is a **pointer**
  - Most likely want to **copy** the **data** that the **pointer points to**
    - Involve creating **new memory buffer** in the **new object**.

## Conversion between types

- Can **perform conversions**
  - In math $\to$ you **define a vector** that *represents* *direction* so the line drawn between **two points is a vector**.
- For **our code** $\to$ already defined a **point class** and **cartesian_vector object**

```c++
    class cartesian_vector 
    { 
        double x; double y;  
    public: 
        cartesian_vector(const point& p) : x(p.x), y(p.y) {} 
    };
```

- There is a **problem here** 
  - We address later, conversions called as such:

```c++
    point p(10, 10); 
    cartesian_vector v1(p); 
    cartesian_vector v2 { p }; 
    cartesian_vector v3 = p;
```

## Making friends

- Problem with previous code $\to$ *cartesian_vector* class accesses **private members**
  - Point class.

```c++
    class cartesian_vector; // forward decalartion 

    class point 
    { 
        double x; double y; 
    public: 
        point(double x, double y) : x(x), y(y){} 
        friend class cartesian_point; 
    };
```

- As **cartesian_vector** is *declared* *after* the **point class**
  - must provide a **==forward declaration==** that *tells* the **compiler** that the **name cartesian_vector** is *about to be used* and will be declared elsewhere.
- **==friend==** $\to$ indicates the **code** for the **entire class** of *cartesian_vector* $\to$ can have **access** to the **private members** *of point class*.

---

- Can declare **friend functions**
  - Example $\to$ could **declare an operator** such that a **point object** can be *inserted* into the **cout object**
    - Therefore printed on to the console.
- Can **not change** the *ostream class* but can define a **global method**

```c++
    ostream& operator<<(ostream& stm, const point& pt) 
    { 
        stm << "(" << pt.x << "," << pt.y << ")"; 
        return stm; 
    }
```

- The function **accesses** the *private members* of *point* so you have to **make the function** a *friend* of the **point class** 
  - With:

```c++
   friend ostream& operator<<(ostream&, const point&);
```

- such a **friend declarations** have to be *declared* in the **point class**
  - But **its irrelevant** whether it is **put** in the **public / private** section.

---

## Marking constructors as explicit

- For some cases $\to$ you **do not want** to allow **implicit conversion** between **one** type that is passed as **parameter** of the **constructor** of *another type*
  - Mark the **constructor** as ***==explicit==*** *specifer*
    - This means that the **only way** to *call* the **constructor** is *using the parentheses* syntax $\to$ **Explicitly calling** the *constructor*

---

- In the **following code** $\to$ you *cannot implicitly* convert a **double** to an **object** of *myType*

```c++
    class mytype  
    { 
    public: 
        explicit mytype(double x); 
    };
```

- Now you have to **explicitly** call the **constructor** if you **want to create an object** with a **double parameter** 

```c++
    mytype t1 = 10.0; // will not compile, cannot convert 
    mytype t2(10.0);  // OK
```

## Destructing objects

- When an **object is destroyed,** a special method called the **destructor** is called. 
- This method has the **name of the class** prefixed with a **~ symbol** and it **does not return a value**.

- If the **object** is an **automatic variable**, on the **stack**, then it will be **destroyed when the variable goes out of scope**.
-  When an **object is passed by value**, a **copy** is made on the **called function's stack** and the **object** will be **destroyed** when the **called function** completes. 
- Furthermore, it **does not matter** **how** the **function completes**, whether an **explicit call** to **return** or **reaching the final brace**, or if an **exception** is thrown; in **all of these cases, the destructor is called.**
-  If there are **multiple objects** in a function, the destructors are **called in the reverse order** to the **construction of the objects** in the **same** **scope.** 
- If you **create an array of objects,** then the **default constructor** is called for **each object** in the array on the statement that declares the array, and all the **objects will be destroyed**--**and the destructor on each one is called**, when the array goes out of scope.

---

```c++
 void f(mytype t) // copy created 
    { 
        // use t 
    }   // t destroyed 

    void g() 
    { 
        mytype t1; 
        f(t1); 
        if (true) 
        { 
            mytype t2; 
        }   // t2 destroyed 

        mytype arr[4]; 
    }  // 4 objects in arr destroyed in reverse order to creation 
       // t1 destroyed
```

- Example for myType class.

- Interesting action occurs on **returning an object**
  - Following annotation what you expect

```c++
    mytype get_object() 
    { 
        mytype t;               // default constructor creates t 
        return t;               // copy constructor creates a temporary 
    }                           // t destroyed 

    void h() 
    { 
        test tt = get_object(); // copy constructor creates tt 
    }                           // temporary destroyed, tt destroyed
```

- The **process** is *more streamlined*
  - For a **debug build** $\to$ compiler see that the **temporary object created** on the *return* of *get_object* function
    - is the **object** used as the **variable** *tt*.
    - So there is **no extra copy** on the **return value** of the *get_object* function

- Function should look like this

```c++
    void h() 
    { 
        mytype tt = get_object();  
    }   // tt destroyed
```

- The compiler is **able to optimize code further**
  - In a **release build**
    - with optimisations enabled $\to$ the **temporary** is **not created** and the *object tt* in the **calling function** will be the **actual object** created
- An **object** will be **destroyed** when you *explicitly delete a pointer* to an object allocated **on the free store**
  - For this case $\to$ the call to the **destructor** is **deterministic**
    - Its called when your code calls **delete**
- For the same class shown below.

```c++
    mytype *get_object() 
    { 
        return new mytype; // default constructor called 
    } 

    void f() 
    { 
        mytype *p = get_object(); 
        // use p 
        delete p;        // object destroyed 
    }
```

- Times when you need the **deterministic** aspect of **deleting an object**
  - With the possible danger of **forgetting** to *call delete*.
- Times when you **prefer** to have **reassurance** that an object is **to be destroyed** at an **appropriate time**
  - With pot that it may be **much later in time**.

---

- If some **data member** in a class is a **custom type** with a **destructor**
  - When the **containing object** is *destroyed* the **destructors** on the **container objects called also** 
    - Not this is only if the **object** is a **class member**
- If some **class member** is a **pointer to an object** in the **free store**
  - Then you must **explicitly delete** the *pointer* in the **containing objects destructor**
    - Must know where the **object** the **pointer points to** is because, as if its **not in free store** , or used by **other objects**
      - Calling **delete** is an **issue**.

## Assigning objects

- The **assignment operator** $\to$ called when *already created object* is *assigned* the **value** of **another one**
  - Default $\to$ get a **copy assignment** operator that *copies* **all the data members**
    - This is **not what you** *want* always $\to$ more so if the **object** has a **data member** that is a **pointer**
      - For this case $\to$ intention **more likely** to do a **deep copy** and **copy** the **data pointed** to *rather* than the **value** of the **pointer**
        - Latter $\to$ two objects point to **same data**.

---

- Defining a **copy constructor**
  - Get the **default copy assignment operator** 
    - makes sense if **you** regard it **important** to *write your own* **copy constructor**
    - Should give some **custom copy assignment operator**
- Defining a **copy assignment operator** $\to$ you get **default copy constructor** unless you *define it*.

---

- The *copy assignment operator* is often a **public member** of the *class*
  - Takes a **const reference** to the *object* that is used to **provide values** for the *assignment*.

- The **semantics** of the *assignment operator* are **that** you can **chain them**
  - Example $\to$ code below calls **assignment operator** on **two of the objects**.

```c++
    buffer a, b, c;              // default constructors called 
    // do something with them 
    a = b = c;                   // make them all the same value 
    a.operator=(b.operator=(c)); // make them all the same value
```

- Final **two lines** do the *same thing*
  - First one **more readable**.
    - To *enable* these *semantics* $\to$ the **assignment operator** must *return a ==reference==* to the **object** that **has been assigned**
      - Therefore the *class* **buffer** has the **following method**:

```c++
    class buffer 
    { 
        // data members 
    public: 
        buffer(const buffer&);            // copy constructor 
        buffer& operator=(const buffer&); // copy assignment 
    };
```

- The **copy constructor** and **copy assignment** appear *similar*
  - The **key difference** $\to$ a **copy constructor** will *create a new object* that *did **not exist*** before the call
    - The **calling code** $\to$ is *aware* that if the **construction fails** $\to$ an *exception* is **raised**.
- For **assignment** $\to$ the objects **already exists** $\to$ just copying the **value** from **one object** to *another*
  - Treated as an **==atomic action==**
    - And the **entire copy** must be performed.
      - can not **fail halfway through** resulting in some **partial object** that is a *bit of both of the objects*.

---

- In **construction** $\to$ an *object* only **exists** after the **construction** is *successful*
  - Therefore a **copy construction** *can* **not happen** on an *object itself*
    - Its perfectly **legal** (*if useless*) $\to$ code to **assign** some *object* to **itself**
      - The **copy assignment** will *check* for this and **take correct action**

---

- One strategy $\to$ called the **copy-and-swap** 
  - using the standard library function **swap** function marked as **noexcept** (*won't throw an exception*)
    - The *idiom* involves:
      1. *Creating* an **temporary copy** of the object on the **RHS** of *assignment*.
      2. **Swapping** its *data members* with the **data members** of the *object* on **LHS**.

## Move semantics

- C++11 provides **move semantics**
  - Through some **move constructor** and **move assignment operator**
    - Are called when a **temporary object** used to **either to create another object** or to be **assigned** to some *existing object*.
- Both cases $\to$ as the **temp object** is *not living* beyond the **statement**, the *contents* of the **temporary** can be **moved** to the **other object**
  - leaving the **temporary object** in some **invalid state**.
    - The **compiler** will *create* these functions for you via **default action** of *moving the data* from the **temporary** to **newly created object** (or assigned to object).

---

- Can **write your own versions**
  - To Indicate  **move semantics** , these have a **parameter** that is **an rvalue reference &&**

> - *If you want the compiler to provide you with a default version of any of these methods, you can provide the prototype in the class declaration suffixed with* =default*.*
> -  In most cases, this is self-documenting rather than being a requirement, but if you are writing a POD class you must use the default versions of these functions, otherwise is_pod *will not return* true*.*

- If you **want to use** only the **move** and **never** to **use copy**
  - Example $\to$ a **file handle class**.
    - Then you **can delete the copy functions**.

```c++
    class mytype 
    { 
        int *p; 
    public: 
        mytype(const mytype&) = delete;             // copy constructor 
        mytype& operator= (const mytype&) = delete; // copy assignment 
        mytype&(mytype&&);                          // move constructor 
        mytype& operator=(mytype&&);                // move assignment 
    };
```

- Class has a **pointer data member** and allows for **move semantics**
  - In which case $\to$ the **move constructor** will be called with **reference** to a **temporary object**.
    - As the object is **temporary** $\to$ will **not survive** after the **move constructor call**
      - means $\to$ the **new object** can *move* the **state** of the **temporary object** into *itself.*

```c++
    mytype::mytype(mytype&& tmp) 
    { 
        this->p = tmp.p; 
        tmp.p = nullptr; 
    }
```

-  The **move constructor** assigns the **temp** objects pointer to **nullptr**
  - So any **destructor defined** for the *class* does **not attempt** to *delete the pointer*.

## Declaring static members

- Declare some **member** of a class $\to$ **a data member** or a **method** as **==static==**
  - Similar to the **static keyword** on *automatic variables* and **functions** declare at **file scope**
    - There are **alternative properties** for this keyword used on **class member**.

## Defining static members

- Using **static** on some class members $\to$ the **item** is **associated** with the *entire class* and **not a single instance**
  - For **data members** $\to$ means there is **one data item** shared by **all instances** of a class
    - A **static method** $\to$ is **not attached** to *one object*
      - Its not **__thiscall** and has **no this pointer**.

---

#### Static method

- A **static method** is part of the **namespace** of some class
  - Can **create objects** for the *class* and access **private members**
- A **static method** $\to$ has the **__cdecl** *calling convention* as default
  - Can declare as **__stdcall** however. 
    - Means you can **write a method** that is *within a class* and use to **init C-like pointers** that are *used* by **many libraries**
- The **static function** can *not call* the **non static methods** on the *class* as a **non static method** requires a **this pointer**
  - The **non statics** can **calls static of course**.
- **non static method** called *through an object*
  - Using the **dot operator** for some *class instance* 
  - Or the **-> ** operator for **object pointer**.
- A **static method** $\to$ does *not need* an **associated object**
  - Can however be **called via one**
    - **2 WAYS** to *call static methods*:
      1. Through object
      2. Through class name

---

```c++
    class mytype 
    { 
    public: 
        static void f(){} 
        void g(){ f(); } 
    };
```

- Class defines a **static method** called `f` 
  - With **non static method g**
    - non static can call static, but not vice versa
      - as static method is **public**, code **outside** the *class can call it*.

```c++
    mytype c; 
    c.g();       // call the nonstatic method 
    c.f();       // can also call the static method thru an object 
    mytype::f(); // call static method without an object
```

- Do **not need to create objects** to even *call* as shown in the **final line.**

---

- Static **data members** need a **bit more work** 
  - When using **static** $\to$ indicates the **members** are *not part of the object*
    - Often $\to$ the **data members** are *allocated* when an **object is created**
- Must **define** the *static data members* **outside** the *class*.

```c++
    class mytype 
    { 
    public: 
        static int i; 
        static void incr() { i++; } 
    }; 

    // in a source file 
    int mytype::i = 42;
```

- The **data member** is *defined outside* of the **class** at **file scope**
  - Named using **class name**.
    - note $\to$ also must be **defined using the type**.
- This case $\to$ the **data member** is **initialized** with a *value*
  - If you **dont do this** 
    - On the **first use** of the *variable* $\to$ have **default value** of the *type* (for this case $\to$ **zero**).
- Choose to **declare** the *class in a **header file***
  - Very common $\to$ definition of **static data members** must be *in the source file.* 
- Can declare a **variable** in a **method** that is *static*
  - For this case $\to$ the **value** is *maintained* across **method calls** in *all objects*
    - So it has the **same effect** as a *static class member*
      - but you **do have issue** when defining **variables outside of the class**.

---

## Static / Global objects

https://stackoverflow.com/questions/29552037/c-declaring-a-static-object-in-a-class

https://www.bogotobogo.com/cplusplus/statics.php

- A **static** *variable* in a **global function** created at **some point** *before* the **function** is *first called*
  - Similar $\to$ a **static object** that is a **member of a class** will be *init* at **some pointer** before it is **first accessed**.

---

- **Static  / Global** objects are **constructed** *before* the **main function** is *called*
  - Then **destroyed** after the **main function ends**
- The *order of initialization* has issue
  - C++ standard $\to$ states that the **initialization** of *static* and *global objects* defined in a **source file** occur *before* any **function / object** defined in that **source file** is *used* 
    - If there are **several global objects** in some *source file* with **static objects** in **each**.
      - There is **no guarantee** on what *order* they are **initialized** 
- Becomes more **prevalent** if one **static object** depends on **another static object**
  - You **cannot guarantee** that the *dependent object* is created **after the object it depends on**.

## Named constructors

- An application of **public static methods**
  - Since the **static method** is a *member of the class*
    - It has **access** to the **private members** of an *instance* of the **class**
      - Therefore **such a method** can *create an object* / perform **additional initialization** 
        - Then *return object to caller* $\to$ called a **==factory method==** https://stackoverflow.com/questions/5120768/how-to-implement-the-factory-method-pattern-in-c-correctly
- The **point class** so far constructed using **cartesian points**
  - Can also **create** a **point based** on **polar coordinates**
    - Where x and y are calculated as such:

```c++
    x = r * cos(theta) 
    y = r * sin(theta)
```

- `r` is the **length** of the **vector** to the point
  - Theta is the angle of this vector **counter clock wise** to `x axis`

- The **point class** has a *constructor* that takes **two double values**
  - Therefore we **cannot use** this to **pass polar co-ordinates**
    - Rather just use a **static method** as a *named constructor*.

```c++
    class point 
    { 
        double x; double y; 
    public: 
        point(double x, double y) : x(x), y(y){} 
        static point polar(double r, double th) 
        { 
            return point(r * cos(th), r * sin(th)); 
        } 
    };
```

- Method called as such:

```c++
    const double pi = 3.141529; 
    const double root2 = sqrt(2); 
    point p11 = point::polar(root2, pi/4);
```

- The object p11 is the **point with the Cartesian co-ordinates** of (1,1). 
- In this example the **polar method calls a public constructor**, but it has **access to private members**, so the **same method** could be written **(less efficiently) as:**

```c++
    point point::polar(double r, double th) 
    { 
        point pt; 
        pt.x = r * cos(th); 
        pt.y = r * sin(th); 
        return pt; 
    }
```

## Nested classes

- Can define classes **within other classes**
  - If the **nested class** is declared **public**
    - Then you **create objects** in the **container class** then **return** *to external code*

- Often $\to$ want to **declare a class** that is **used** by the *class* and should be **private**
  - Following declares a **public nested class**:

```c++
    class outer 
    { 
    public: 
        class inner  
        { 
        public: 
            void f(); 
        }; 

        inner g() { return inner(); } 
    }; 

    void outer::inner::f() 
    { 
         // do something 
    }
```

- See how the **name of the nested class** is *prefixed* with name of containing class.

## Accessing containing objects

- Most **frequent use** of const is with **reference** as a **function parameter** to tell the compiler that the **function** has **read only access**
  - Such a **const reference** is used so that **objects** are *passed by reference* to **avoid** the *overhead of the copying* that occurs if the **object** were passed **by value**. 

- Methods on a **class** can be **access** the *object data members* and possibly **change them** 
  - Therefore if you **pass an object** through a **const reference**
    - The *compiler* only allow the **reference** to *call methods* that **dont change the object**
      - The **point class defined above** $\to$ had **two accessors** to **access data** within the *class.*

```c++
    class point 
    { 
        double x; double y; 
    public: 
        double get_x() { return x; } 
        double get_y() { return y; } 
    };
```

- Defining a function that **takes a const reference** to this and **you attempt** to *call the accessors*
  - Obtain an **error from the compiler**

```c++
    void print_point(const point& p) 
    { 
        cout << "(" << p.get_x() << "," << p.get_y() << ")" << endl; 
    }
```

- Error from the compiler is **bit obscure**

```c++
cannot convert 'this' pointer from 'const point' to 'point &'
```

- The message is the **compiler complains** that the **object is const** therefore **immutable**
  - And **does not know** whether *these methods* will **preserve** the *state of the object*
    - The **solution** is simply just to add **const keyword** to *methods* that **dont change the object state**.

```c++
    double get_x() const { return x; } 
    double get_y() const { return y: }
```

- This means the **this pointer** in itself is **const**
  - The **const keyword** is part of the **function prototype** $\to$ therefore **the method** can be *overloaded* on this
- Can have **one method** that is *called* when it **is called** on a *const object* and another **called** on a *non-const object*. 
  - Therefore enables you to **implement** a *==copy on write pattern==* 
    - example $\to$ a **const method** would *return read only access* to the **data** and the **non const method** would return a **copy of the data that is writeable**.

---

- A method **marked** with **const** must *not alter data members*

  - Not even **temporarily**.
    - Therefore a **method** can *only call const methods*.

- Can be **rare cases** when a *data member* is **designed** be *changed* through some **const object**

  https://stackoverflow.com/questions/105014/does-the-mutable-keyword-have-any-purpose-other-than-allowing-the-variable-to

  - For this **declaration** of the **member** is *marked* with the **mutable** keyword.

---

## Using objects with pointers

- Objects can be **created** on the **free store** and *accessed* through **a typed pointer**
  - Gives **more flexibility** as its efficient to **pass pointers** to functions
    - Can **explicitly** determine the **lifetime** of the **object** as an **object** is **created** with the *call* to **new** and **destroyed** by the call to **delete**.

## Getting pointers to object members

- If you **need to gain access** to the **address** of a **class data member** through **some instance**
  - Assuming the data member is **public** $\to$ simply use **&** operator:

```c++
    struct point { double x; double y; }; 
    point p { 10.0, 10.0 }; 
    int *pp = &p.x;
```

- For this case **struct** used to **declare** *point* so the members are **public** as *default*
  - The **second line** uses an **initialisation**  *list* to construct a **point** object of **two values**
    - Then the **final line** gets a **pointer** to *one of the data members*
      - Of course $\to$ pointer **cannot** be used **after** the *object* has **been destroyed**.
- Data members $\to$ are **allocated in memory** (*for this case* $\to$ on the **stack**)
  - Therefore the **address operator** just gets a **pointer to that memory**.

---

- Function **pointers** $\to$ are **different cases** 
  - There is only **one copy** of the **method in memory**, regardless of **how many instances** of the *class are created*.
    - As methods are **called using** the **__thiscall** convention (with **hidden this parameter**).
      - Must define a **function pointer** that can be **initialized** with a **pointer** to an *object* to provide the **this pointer**
        - Consider this **class**:

```c++
    class cartesian_vector 
    { 
    public: 
        // other items 
        double get_magnitude() const 
        { 
            return std::sqrt((this->x * this->x) + (this->y * this->y)); 
        }  
    };
```

- Define a **function pointer** to *get_magnitude* as such:

```c++
    double (cartesian_vector::*fn)() const = nullptr; 
    fn = &cartesian_vector::get_magnitude;
```

- The **first line** $\to$ declares **function pointer**
  - Similar to the **C function** pointer **declarations** except inclusion of the **class name** within the **pointer type**
    - Needed so the **compiler knows** that it must provide a **this pointer** in *any* call via this **pointer**
- The **second line** $\to$ obtains **pointer to the method**.
  - There is **no object involved** $\to$ you are *not getting* some **function pointer** to a **method** on an **object**
    - You obtain a **pointer** to a **method** on *a class* that must be **called** through **an object**
      - To **call this method** via **this pointer** $\to$ use the **pointer** to *the member operator* `.*` on an object

``` c++
    cartesian_vector vec(1.0, 1.0); 
    double mag = (vec.*fn)();
```

- First line **creates an object**
- Second **calls the method**
  - The *pointer* to the **member operator** says that the **function pointer** on the *right* is called with the **object on the left**.
    - The **address** of the *object* on the **left** is used for **the this pointer** when the **methods called**.
      - As its a **method** $\to$ must provide **parameter list**
        - Which in this case is **empty** (if there are parameters $\to$ there would be in the **pair of parentheses**)
- If there is an **object pointer** $\to$ the *syntax* is **similar**
  - However use the **->*** pointer to the **member operator**

```c++
    cartesian_vector *pvec = new cartesian_vector(1.0, 1.0); 
    double mag = (pvec->*fn)(); // alt : (*(pvec).*fn)
    delete pvec;
```

## Operator overloading

- One of behaviours of a **type** is the **operations you can apply to it**. 
- C++ allows you to **overload the C++ operators** as part of a **class** so that it's **clear that the operator is acting upon the type**. 
- This means that for a **unary operator** the **member method** should **have no parameters** and for a **binary operator** you need only **one** **parameter,** since the **current object will be on the left of the operator**, and **hence the method parameter is the item on the right**. 
- The following table summarizes how to implement unary and binary operators, and four exceptions:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210706115037358.png)

---

- The **square symbol**
  - used to **indicate** that *any* of the **acceptable** *unary* or *binary* operators except for **the four operators** mentioned in the table.

---

- There **no strict rules** over what *operator* should return
  - Helps if some **operator** on a *custom type* behaves like the **built in type**
    - Must be **consistency** $\to$ if you *implement* the `+` operator to **add two objects**
    - The same plus action should be used in `+=` 
- Argue that the **plus action** will *determine* what the **minus action** should be like
  - Hence the `-` and `-=` operators 
    - Similar $\to$ defining the **< operator** , should define the **<= , > , >=, ==, !=**

---

- The **standard library's algorithm**
  - Such as *sort* $\to$ only expect the **< operator** to be **defined** on a **custom type**

---

- Table **shows** you can **implement** almost *every operator* as either **member / custom type class / global function** (exception being four listed to have **member methods.**) 
  - In general $\to$ best to **implement** the **operator** as *part* of the **class** as it **maintains encapsulation**
    - The *member function* has **access** to the **non public members of the class**.

---

-  A **unary operator**
  - is the **unary negative operator** as *example*
    - This often *does not alter* an **object**, but just **returns a new object** that is the *negative* of that *object*
- For the **point class**
  - Means $\to$ making both **coordinates negative**
    - This is **equivalent** to a *mirror* of the **cartesian point** in a line **y = -x**:

```c++
    // inline in point 
    point operator-() const 
    { 
        return point(-this->x, -this->y); 
    }
```

- Operator declared as **const** as its clear the **operator** does **not change the object**
  - Therefore its safe to be **called on const object**
    - Operator called as such

```c++
    point p1(-1,1); 
    point p2 = -p1; // p2 is (1,-1)
```

- To understand **why we implemented** the *operator as such*
  - check what the **unary operator** would do when *applied* to some **built in type**
    - The **second statement** $\to$ `int i, j = 0, i = -j` will **only alter** `i` and **not alter** `j` 
      - Therefore the **member `operator-`** should **not affect** the *value* of the **object**.

---

- The **binary negative** has **different meaning**
  - It has **two operands**
  - The *result* is **different type** as its **result** is a **vector** that *indicates* some **direction** by *taking*  **one point** away from **another**
- Assuming the **cartesian_vector** is *already defined* with a **constructor** that has **two parameters**
  - We write:

```c++
    cartesian_vector point::operator-(point& rhs) const 
    { 
        return cartesian_vector(this->x - rhs.x, this->y - rhs.y); 
    }
```

- The **increment / decrement** operators have **special syntax**
  - They are **unary operators** $\to$ can be **prefixed / postfixed**
    - and they **alter** the **object** they are **applied to**
- Major **difference** of the operators is that **post fix** will *return the value* of the **object** *before* the **increment / decrement** action
  - Therefore a **temporary** *must be created*
    - This reason $\to$ the **prefix operator** has **better performance**
- In some **class definition** $\to$ to *distinguish* between the **two**
  - The **prefix operator** has *no parameters* and the **postfix operator** has a *dummy parameter*
    - in the **preceding table, o is given.**  
      - Example $\to$ with **my type class**

```c++
    class mytype  
    { 
    public: 
        mytype& operator++() 
        {  
            // do actual increment 
            return *this; 
        } 
        mytype operator++(int) 
        { 
            mytype tmp(*this); 
            operator++(); // call the prefix code 
            return tmp; 
        } 
    };
```

- Actual **increment code** is implemented in **prefix operator**
  - This logic used by **postfix operator** via some **explicit call to the method**.

## Defining function classes

- A **functor** is class that **implements** the **() operator**
  - Therefore you can *call an object* using the **same syntax** as a *function*

```c++
    class factor 
    { 
        double f = 1.0; 
    public: 
        factor(double d) : f(d) {} 
        double operator()(double x) const { return f * x; }  
    };
```

- Called as such

```c++
    factor threeTimes(3);        // create the functor object 
    double ten = 10.0; 
    double d1 = threeTimes(ten); // calls operator(double) 
    double d2 = threeTimes(d1);  // calls operator(double)
```

- Code shows that the **functor object** not only provides **behaviour**, but can also **have a state**
  - In this case $\to$ **performing an action** on the **parameter**
- Preceding two lines are *called via operator()* on an object.

```c++
    double d2 = threeTimes.operator()(d1);
```

- The **functor object** is *called* as if it **is a function declared like this**

```c++
    double multiply_by_3(double d) 
    { 
        return 3 * d;  
    }
```

- Imagine that you want to **pass a pointer** to a **function**
  - Perhaps $\to$ you want the **functions behaviour** to be *altered by **external code***
    - To be **able** to **use** either a **functor** or a **method pointer** must *overload your function*

```c++
    void print_value(double d, factor& fn); 
    void print_value(double d, double (*fn)(double));
```

- First takes a **reference** to the **functor object**
- Second has **C-type** *function pointer* (which you **can pass** a *pointer* to **multiply_by_3**)
  - and is **quite unreadable.**
    - Both cases $\to$ the **fn** *parameter* is *called* in the **same way** in the **implementation code**
      - Need to **declare two** functions as they are **different types**
- Consider the **magic of function templates**

```c++
    template<typename Fn> 
    void print_value(double d, Fn& fn) 
    { 
        double ret = fn(d); 
        cout << ret << endl; 
    }
```

- This is **generic code**
  - The **Fn** type can be a **C function pointer** or a **functor** *class*
    - The **compiler** will **generate** the **appropriate** *code*

> - This code can be called by either passing a function pointer to a global function, which will have the __cdecl *calling convention, or a functor object where the* operator() *operator will be called, which has a* __thiscall *calling convention.*

- This is just **implementation detail**
  - It does mean you can write **a generic function** that takes either **C-like** function pointer or **functor object** as **parameters**
- The **C++** standard library uses this **magic**
  - Means that the **algorithm** it provides can be **called** with either a **global function** / **functor** / **lambda expression**

---

- The **standard library** uses **three type** of *functional classes*
  1. Generators
  2. Unary and Binary functions (functions with zero, one or two parameters)
- Additionally $\to$ the **standard library** calls a **function object** (*unary or binary*)
  - That returns a **boll predicate**
- The *docs* tell you if some **predicate / unary / binary** function is **required**
  - Older version of **SL** need to know the **types** of the *return value and parameters*
    - of the **function** in order to work.
      - For this reason $\to$ **functor classes** had to be **based upon standard classes**. `unary_function` and `binary_function`
- C++11 $\to$ this **requirement** was *removed*
  - Therefore there is **no requirement** to **use these classes**.

---

- Some cases $\to$ want to use **binary functor** when a **unary functor** is required
  - Example $\to$ the **standard library defines** the *greater class* that, when used as a **function object**
    - Takes **two parameters** and a **bool** to *determine* whether the **first parameter** is *greater than the second one*
      - using the **operator>** defined by the **type of both parameters**
        - This is used for **functions** that require a **binary functor** therefore the **function** will  *compare two values*
- Example:

```c++
    template<typename Fn>  
    int compare_vals(vector<double> d1, vector<double> d2, Fn compare) 
    { 
        if (d1.size() > d2.size()) return -1; // error 
        int c = 0; 
        for (size_t i = 0; i < d1.size(); ++i) 
        { 
            if (compare(d1[i], d2[i])) c++; 
        } 
        return c; 
    }
```

- Takes **two collections** and compares the **corresponding items** using the **functor** passed as the **last parameter**
  - Can be **called as such**:

```c++
    vector<double> d1{ 1.0, 2.0, 3.0, 4.0 }; 
    vector<double> d2{ 1.0, 1.0, 2.0, 5.0 }; 
    int c = compare_vals(d1, d2, greater<double>());
```

- The **greater functor** class defined in `<functional>` header
  - Compares **two numbers** using **operator>** defined for the **type**
    - What if you wish to **compare** items in a container with a **fixed value**
      - That is $\to$ when the **`operator()(double, double)`** method on **some functor** is *called*
        - One parameter has a **fixed value**
          - One option $\to$ to define a **stateful functor class** so the *fixed value* is a **member** of the **functor object**
          - Another option $\to$ fill another `vector` with the **fixed value** and *continue* **to compare** two `vector` (expensive for **large vectors**).
- Best option is to **reuse the functor class**
  - but to just **bind** a *value* to **one of the parameters**
    - A version of the **compare_vals** functions can be **written** like this to take **just one vector**

```c++
    template<typename Fn>  
    int compare_vals(vector<double> d, Fn compare) 
    { 
        int c = 0; 
        for (size_t i = 0; i < d.size(); ++i) 
        { 
            if (compare(d[i]) c++; 
        } 
        return c; 
    
```

---

- The code is **written** in order to **call** the **functor parameter** on just **a single value**
  - Its assumed that the **functor object** contains the **other value** to *compare*
    - Carried out **by binding** *the functor class* to the **parameter** https://en.cppreference.com/w/cpp/utility/functional/bind



https://stackoverflow.com/questions/6610046/stdfunction-and-stdbind-what-are-they-and-when-should-they-be-used

```c++
    using namespace std::placeholders; 
    int c = compare_vals(d1, bind(greater<double>(), _1, 2.0));
```

- The **bind** function is *variadic*
  - The **first parameter** $\to$ is the **functor** *object* and it **is followed** by the **parameters** that are *passed* the **operator()** method of the functor
- The **compare_vals** function is **passed** to the **functor** at *that position* in the **call** to **functor**
  - (here **2.0** is passed to the **second parameter**)
- If a **symbol** preceded by an **underscore**
  - Then its a **placeholder**
    - There are **20 such symbols** (_1 to _20) defined in the **std::placeholders namespace**
      - The *placeholder* means use the **value passed in this position** to the **binder object** *operator()* method to **call the functor call** *operator()* 
        - indicated by the **placeholder** https://en.cppreference.com/w/cpp/utility/functional/placeholders
- Therefore the **placeholder** in this call means $\to$ `pass the first parameter from invoking the binder and pass it to the first parameter of the greater functor operator`
- Previous code compares each item in the **vector** with **2.0**
  - Will keep a **count** of those that are **greater than 2.0**
    - invoke in this way:

```c++
    int c = compare(d1, bind(greater<double>(), 2.0, _1));
```

- The **parameter list** has *swapped* 
  - This means that **2.0** is compared with **each item** in the **vector** 
  - The function keep a count of **how many times** 2.0 is greater than the item.

---

- The **bind function** and *placeholders* are **new** to **C++11**
  - In previous versions you could use **bind1st** and **bind2nd** functions to **bind a value** to *either* the **first / second parameters** of the **functor**

---

## Defining conversion operators

https://stackoverflow.com/questions/1307876/how-do-conversion-operators-work-in-c

https://www.tutorialspoint.com/conversion-operators-in-cplusplus

- Can **already seen** that a *constructor* used to **convert** from **another type** to your **custom type**
  - If your **custom type** has a **constructor** that takes the **type you** are *converting*.
- Can perform the **conversion** in the **other direction**
  - Converting the **object** into *another type* 
    - To do this $\to$ provide an **operator** without a **return type** with *the name* of the **type to convert to**
- In this case $\to$ need a **space** between the **operator keyword** and the **name**

```c++
    class mytype 
    { 
        int i; 
    public: 
        mytype(int i) : i(i) {} 
        explicit mytype(string s) : i(s.size()) {} 
        operator int () const { return i; } 
    };
```

- This code can **convert** an **int** or **a string** to **mytype**
  - In the **latter case** $\to$ only through an **explicit mention of constructor**
    - last line allows you to **convert** an **object** back to an **int**:

```c++
    string s = "hello"; 
    mytype t = mytype(s); // explicit conversion 
    int i = t;            // implicit conversion
```

-  Make such **conversion operators** as *explicit*
  - They are **called only** when an **explicit cast  is used**.
    - Many cases $\to$ want to **leave** off **this keyword** because **implicit** *conversions are useful* when you want to **wrap a resource** in *a class*
      - Then use the **destructor** to do **automatic resource management**.

- Another example of using **conversion operator**
  - *Returning values* from a **stateful functor**
    - Idea is that the **operator()** perform *some action* and the **result** is **maintained** by the **functor**
      - issue $\to$ how do **you obtain** the *state of the **functor*** 
        - Especially if they are created as **temporary objects** $\to$ conversion operator provides this **functionality**

---

- Calculating an **average** $\to$ do in **two stages**
  - The **first stage** $\to$ *accumulate values* 
  - **Second stage** $\to$ calculate the **average** by **dividing** it by the **number of items**
- Following **functor** class does **this with** the **division performed** as part of the **conversion** to a *double*

```c++
    class averager 
    { 
        double total; 
        int count; 
    public: 
        averager() : total(0), count(0) {} 
        void operator()(double d) { total += d; count += 1; } 
        operator double() const 
        {        
            return (count != 0) ? (total / count) : 
                numeric_limits<double>::signaling_NaN(); 
        } 
    };
```

- Called as such:

```c++
    vector<double> vals { 100.0, 20.0, 30.0 }; 
    double avg = for_each(vals.begin(), vals.end(), averager());
```

- The **for each ** function calls the **functor** every item in the **vector**
  - The **operator()** just sums the items passed to it and **maintains count**
- Interesting part $\to$ after the **for_each** function has iterated over **all of the items** in the *vector*
  - It *returns* the **functor** 
    - Therefore is an **implicit conversion** to *double*
      - Calls the **conversion operator** that *calculates the average*.

## Manging resources

- Seen one sort resource that requires **careful management** $\to$ the **MEMORY**
  - Allocate new memory with the **new keyword** and **deallocate** using the **delete keyword**

- A *failure* to **deallocate** causes **memory leaks**
  - Memory is the **most fundamental of system resources** 
    - however most operating system have **many other resources**
      1. File handles
      2. graphic objects
      3. synchronization objects
      4. threads
      5. processes.

- The **possession** of *such a resource* is *exclusive* and **prevents other code** from *accessing the resource accessed* through **such resource**
  - Therefore **important** that *resources* are **freed** at *some point*
    - Usually freed in **quick manner**

---

- Classes help with **Resource Acquisition is Initialization **
  - invented by Bjarne himself
    - Simply $\to$ the **resource is allocated** in the **constructor** of some object and **freed in the destructor**
      - Therefore the **lifetime** of the **resource** is the **lifetime of the object**
- Such a wrapper **object** are *allocated* on the **stack**
  - Therefore you are **guaranteed** that the **resource** are **freed** when the **object** goes **out of scope**
    - Regardless of **how this happens**

---

- If **objects** are declared in the **code block** for a **looping statement** (while , for)
  - at the **end** of **each loop** $\to$ the **destructor** is called **in reverse order of creation** 
    - Then *created again* when the **loop is repeated*
      - Occurs whether the **loop** is repeated because the **end of the code block** has been reached of if the **loop** is repeated through a **call to continue**
- Another method $\to$ leave a **code block ** is via **break** / **goto**
  - Or if the code calls **return** to **leave the function**
    - If the code raises an **exception** $\to$ the **destructor** is called as the **object goes out of scope**
      - So if the code is guarded by a **try block**
        - The **destructor** of *objects* declared in the **block** will be called before the **catch clause is called**
      - If **no guard block** $\to$ the **destructor** called **before** the **function stack** becomes **destroyed and the exception propagated.**

## Writing wrapper classes

https://stackoverflow.com/questions/5307169/what-is-the-meaning-of-a-c-wrapper-class#:~:text=A%20%22wrapper%20class%22%20is%20a,value))%20%7B%7D%20%2F%2F%20note!

- There are **many issues** that must be **addressed** when writing a class **to wrap a resource**
  - Constructor used $\to$ either to
    1. Obtain the **resource** using the **some library function** (often accessed via some **opaque handle**)
    2. Take **resource** as a **parameter**
       - This **resource** is stored as a **data member** so *other methods* $\to$ on the *class* can **use it**
         - **released** in the *destructor* using **whatever function** the **library provides** to *do this*
- This is the **bare minimum**
  - Must **think** on **how the object is used**.

---

- Often, such **wrapper classes** are useful if you **can use instances** as if they are **the resource handle**
  - Meaning that **you maintain** the *same style* of **programming** to *access* the **resource**
    - but you **dont worry** too much about **releasing the resource.**

---

- Think about **whether** you want to be able to **convert** between the **wrapper class** and the **resource handle**
  - If you **do allow** $\to$ means you must **think** about **cloning the resource**
    - So that you dont **have two copies** of the **handle** $\to$ one that is managed by the **class** and the **other copy** that can be **released** by **external code**
- Must consider whether you want to **allow** the **object** to *be copied* / **assigned** and if so
  - Must implement the **copy constructor, move constructor and copy/move assignment operators**

## Using smart pointers

- The C++ Standard Library provides **several classes** to **wrap resources accessed through pointers**. 
- To **prevent memory leaks**, you have to **ensure that memory allocated** on the **free store** is **freed** at some point. 
- The idea of a **smart pointer** is that you **treat an instance** as **if it is the pointer**, so you use the *** operator** to **dereference** to **get access** to the **object it points** to or use the **-> operator** to **access a member** of the **wrapped object**. 
- The **smart pointer** class will **manage the lifetime** of the **pointer it wraps** and will **release the resource appropriately**.

- The **Standard Library** has three smart pointer classes: unique_ptr, shared_ptr, and weak_ptr. 
- Each handles how the resource is released in a different way, and how or whether you can copy a pointer.

---

## Managing exclusive ownership

- The `unique_ptr` constructed with a **pointer** to the **object** that it **maintains**
  - This class provides the **operator** $*$ to give **access** to the **object**
    - Dereferencing the **wrapped pointer**
  - Also provides **->** , so that if the **pointer** is **for a class**
    - You can **access** the **members** through the **wrapped pointer**
- Following **allocates** an **object** on the **free store** then *manually* maintains its **lifetime**

```c++
    void f1() 
    { 
       int* p = new int; 
       *p = 42; 
       cout << *p << endl; 
       delete p; 
    }
```

- Obtain a **pointer to memory** on the **free store** allocated for an **int**
  - To **access** the *memory* $\to$ either to **write to it** or **read from it**
    - You **dereference** the **pointer** with the ***** operator.
      - When finished $\to$ must **call the delete** to *deallocate memory* and **return** it to **the free store**
- Consider the **same code** but with a **smart pointer**.

```c++
   void f2() 
    { 
       unique_ptr<int> p(new int); 
       *p = 42; 
       cout << *p << endl; 
       delete p.release(); 
    }
```

- two **main differences** is the *smart pointer* is **constructed** explicitly via **calling** the **constructor** 
  - Takes a **pointer** of **type** that is used as **template parameter**
    - This pattern **reinforces** the **idea** the **resource** should **only be managed** by the **smart pointer**.
- Second change $\to$ the **memory** is *deallocated* via calling the **release method** on the **smart pointer** object
  - This takes **ownership** of the **wrapped pointer** so we can **delete** it **explicitly.**

- The **==release==** method **is realising** the **pointer** from the *ownership* of the **smart pointer**
  - After the call $\to$ the **smart pointer** will *no longer wrap* around the resource.
    - The `unique_ptr` has a **get method** that obtains **access** to the **wrapped pointer**
      - However the **smart pointer** *object* still **retains ownership** 
        - Do **not delete the pointer** obtained this way.

- A `unique_ptr` **object** will **wrap a pointer** and **just the pointer**
  - Meaning the **object** is the **same size in memory** as the **pointer it wraps**
    - So far $\to$ the **smart pointer** has added **little** 
- Alternative **deallocation method**

```c++
    void f3() 
    { 
       unique_ptr<int> p(new int); 
       *p = 42; 
       cout << *p << endl; 
       p.reset(); 
    }
```

- This is **deterministic releasing** of the *resource*
  - Means that the **resource** is *released* just when you want it to occur
    - Similar situation to with **the pointer**
- Code here $\to$ does **not release** the *resource itself*
  - its **allowing** the **smart pointer to do it** $\to$ uses a **deleter**
    - Default deleter for **unique_ptr** is a **functor class** called **default_delete**
      - Calls the **delete operator** on the **wrapped pointer**.
- If you choose to use **deterministic destruction**
  - *==reset==* is **preferred** 
    - Can **provide** a **personal deleter**  via passing the **type** of a **custom functor class** as the **second parameter** to *unique_ptr* template.

```c++
    template<typename T> struct my_deleter 
    { 
        void operator()(T* ptr)  
        { 
            cout << "deleted the object!" << endl; 
            delete ptr; 
        } 
    };
```

- In the code $\to$ you **specify** what you **want the custom deleter**:

```c++
unique_ptr<int, my_deleter<int> > p(new int);
```

- May need to **carry** out **additional cleanup** before **deleting the pointer**
  - Or the **pointer** could be **obtained** by a *mechanism* other than **new**
    - Can use a **custom deleter** to *ensure* the **correct releasing** function is **called**

- The **deleter** is **part** of the **smart pointer class**
  - If there are **two different smart pointers** that are using **two different delete this way**
    - The **smart pointer types** are *different even* if the **wrap** the **same type of resource.**

> - *When you use a **custom deleter**, the size of a* **unique_ptr** object may be **larger than the pointer wrapped**. 
> - *If the **deleter is a functor object**, **each smart pointer** object will **need memory** for this, but if you use a ==lambda expression==, no more extra space will be required.*

- Most likely to **allow the smart pointer** to *manage* the **resource lifetime** for you
  - To do this $\to$ just allow the **smart pointer** object to **go out of scope**.

```c++
    void f4() 
    { 
       unique_ptr<int> p(new int); 
       *p = 42; 
       cout << *p << endl; 
    } // memory is deleted
```

- As the **pointer** that has been created is a **single object**
  - Means $\to$ you can **call** the **new operator** on an *appropriate constructor* to **pass** in **initialization parameters**.
    - The **constructor** of `unique_ptr` is **passed** a **pointer** to an **already constructed object**
      - The class manages the **lifetime** of the **object after that**, 
      - even if a `unique_ptr` can be created **directly** via calling **its constructor**
        - You **cannot** call the **copy constructor** therefore you **cannot** use the **initialization** syntax during **construction**
          - Rather $\to$ the **standard library**  provides a **function** called `make_unique`. 
- this has **several overloads**
  - For this reason $\to$ preferred way to **create smart pointers** based on this class.

```c++
    void f5() 
    { 
       unique_ptr<int> p = make_unique<int>(); 
       *p = 42; 
       cout << *p << endl; 
    } // memory is deleted
```

- Code will **call** the **default constructor** on the **wrapped type (int)**
  - Can *provide* **parameters** that will **be passed** to the **appropriate** constructor of the **type**
    - Example $\to$ a **struct** that has a **constructor** with **two parameters** $\to$ following used.

```c++
    void f6() 
    { 
       unique_ptr<point> p = make_unique<point>(1.0, 1.0); 
       p->x = 42; 
       cout << p->x << "," << p->y << endl; 
    } // memory is deleted
```

- The `make_unique` function **calls** the **constructor** that *assigns* the **members** with **non default values**
  - The `->` operator will **return** a **pointer** and the **compiler** will **access** the **object members** through this pointer.

---

- There is a **specialization** of `unique_ptr` and `make_unique` for arrays
  - The **default deleter** for this version of `unqiue_ptr` calls **delete[]** on the **pointer**
    - Therefore deletes **every object** in the **array** (and call **each destructor**)

- The class **implements** an **indexer operator** []
  - So you can **access each item** in the **array**
    - However $\to$ there are **no range checks**, therefore like a **built in array variable** $\to$ can **access** beyond the **end of the array**.
      - There are **no dereferencing operators** (* or ->), therefore a **unique_ptr** object based on **an array** can *only* be **accessed** with **array syntax**.

- The `make_unique` function has an **overload** that allows you to **pass the size** of the array to create
  - But you must **init** each **object individually**
- https://stackoverflow.com/questions/37514509/advantages-of-using-stdmake-unique-over-new-operator

```c++
    unique_ptr<point[]> points = make_unique<point[]>(4);     
    points[1].x = 10.0; 
    points[1].y = -10.0;
```

- Creates an **array** with *four* point objects set to **default value**
  - The **following line** *initializes* the **second point** to a value of **(10.0, -10.0)**
    - It is **almost always** better to use **vector** or **array** than `unique_ptr` to **manage arrays of objects**.

> - *Earlier versions of the C++ Standard Library had a smart pointer class called* auto_ptr*.*
> - This was a first attempt, and worked in most cases, but also had some limitations; for example, auto_ptr *objects could not be stored in Standard Library containers. C++11 introduces rvalue references and other language features such as move semantics, and, through these,* unique_ptr objects can be stored in containers.
> - The auto_ptr *class is still available through the* <new> *header, but only so that older code can still compile*

- Vital point to **remember** about the **unique_ptr** is that it **ensures** that there is **one copy** of the **pointer**
  - Important as the **class destructor** will **release** the **resource**
    - So if you **could** copy a **unique_ptr** object $\to$ it would mean **more** than **one destructor** attempts to **release the resource**

- Objects of **unique_ptr** have **exclusive ownership**
  - An **instance** always **owns** what it **points to**

---

- You **cant copy assign** the **smart pointers**
  - (the copy assignment operator and copy constructor are deleted)
    -  You can however **move** them via **transferring ownership** of the **resource** from the **source pointer** to **destination pointer**.
- Therefore a **function** can **return** a `unique_ptr` as the **ownership** is **transferred** through **move semantics** to the **variable** that is **being assigned** to the *value* of the **function**
  - If the **smart pointer** is **placed** in a **container** there is **another move.**

## Sharing ownership

https://docs.microsoft.com/en-us/cpp/cpp/how-to-create-and-use-shared-ptr-instances?view=msvc-160

https://stackoverflow.com/questions/7657718/when-to-use-shared-ptr-and-when-to-use-raw-pointers

- There are **occasions** you need to **share a pointer**
  - may create **several objects** and pass a **pointer** to some **single object** to *each of them*
    - So that they **can call this object**
- When some **object** has a **pointer** to *another object* 
  - That **pointer** *represents* a **resource** that should be **destroyed** during the **destruction** of *the containing object*
    - If a pointer is **shared** $\to$ it *means* that when an **object** *deletes* the pointer
      - The **pointers** in **all** of the **other objects** will be **invalid** $\to$ called a **dangling pointer** (no longer points to an object)

- Need a mechanism where **several objects** can **hold a pointer** that remains **valid** until **all objects** using the **pointer** have *indicated* they **no longer need** to *use it*.

---

- C++11 has a **shared_ptr** 
  - maintains **reference count** on the **resource** $\to$ *each copy* of the **shared_ptr** for **that resource** will *increment the reference the count*
    - decrement on pointer destruction.
- When the **last shared_ptr** object *decrements* the **reference** count to **zero**
  - Safe to **release the resource**

---

- Each **shared_ptr object** has a **pointer** to a **shared buffer** called the **==control block==**
  - This holds the **raw pointer** and **pointer** to **control block**
    - Therefore share_ptr object holds **more data** than the `unique_ptr` 
- The **control block** used for **more** than *just the reference count*

---

- a **shared_ptr** can be created to use **custom deleter**
  - passed as **constructor parameter** 
- The *deleter* is stored in the **control block**
  - This is vital as it means the **custom deleter** is **not** part of the *type* of the **smart pointer**
    - Therefore *several shared_ptr* objects wrapping **same resource type** can use **different deletes**

---

- Create a **shared_ptr** object from **another** *shared_ptr* object.
  - this will **initialize** the *new object* with the **raw pointer** and the **pointer** to *the control block*
    - Then **increment** the *reference count*

```c++
    point* p = new point(1.0, 1.0); 
    shared_ptr<point> sp1(p); // Important, do not use p after this! 
    shared_ptr<point> sp2(sp1); 
    p = nullptr; 
    sp2->x = 2.0; 
    sp1->y = 2.0; 
    sp1.reset(); // get rid of one shared pointer
```

- The **first shared pointer** is created using a **raw pointer**
  - This is *not recommended* way to *use* the **shared_ptr**
- Second shared pointer created using the **first smart pointer**
  - So now **2 shared pointers** (p assigned to **null**).

- Either sp1 or sp2 can be **used to access** the *same resource*
  - At the **end** of the **code** $\to$ one **shared pointer** is *reset* to **nullptr**
    - means that **sp1** is *no longer* has a **reference count** on the *resource*
      - Can **not use** it to **access the resource**
- Can **still use sp2** to *access* the **resource** until it **goes out of scope**
  - or just call **reset**.

---

- In this code $\to$ the **smart pointers** are created from a **seperate** *raw pointer*
  - As the *shared* pointers now have **taken over** the *lifetime management* of the **resource**
    - Important to **no longer** use the **raw pointer**
      - For this case $\to$ assigned **to nullptr**.
- Better to **avoid** the *use of raw pointers*
  - The **standard library** enables this with a **function** called *make_shared* 
    - Can use as such.

```c++
    shared_ptr<point> sp1 = make_shared<point>(1.0,1.0);
```

- The function will **create** the **specified object** using a *call to new*
  - As it takes a **variable number** of *parameters*
    - Can use it to **call any constructor** on the **wrapped class**.

---

- Can create a **shared_ptr** object from a **unique_ptr object**
  - Means that the **pointer** is *moved* to the **new object** and the *reference* counting **control block** is *created*

- As the **resource** will **now be shared**
  - There is *no longer* any **exclusive ownership** on the **resource**
    - Therefore the *pointer* in *unique_ptr* is made a **nullptr**
- Means you can **have** a **factory** function that **returns a pointer** to some **object** wrapped in a **unique_ptr**
  - The *calling code* can **determine** if it **will use** the **unique_ptr** object to gain **exclusive access** to the *resource* or a **shared_ptr** object to **share it**

---

- There is **no point** using *shared_ptr* for **arrays of objects**
  - There are **much better ways** to *store collections* of **objects** (*vector / array*)
    - In any case $\to$ there is an **indexing operator** [] and the **default** deleter calls **delete** and *not delete[]*

---

## Handling dangling pointers

- Delete resource > set pointer to nullptr > check a pointer before using to see if it is nullptr
  - This is so you **do not call** a **pointer** to memory for an **object** that has **been deleted**
    - A **dangling pointer**

---

- There are **situations** where a **dangling pointer** occurs by **design**
  - Example $\to$ a **parent object** may *create* **child objects** that have a **back pointer** to the *parent*
    - If a **child** has *access* to the **parent** (example of this is a window that creates child controls, often useful for the child controls to have access to the parent window).
- Problem $\to$ using a **shared pointer** in *this situation* is that the **parent** will *have* a **reference** count on **each child control**
  - Alongside **each child control** having a **reference count** on the **parent**
    - Creates a **circular dependency**.

---

- Example $\to$ if there is a **container** of **observer objects** with intention of **being able** to *inform each* of the **observer objects**
  - via calling **a method** on each of them
    - Maintaining a **list** can be **complex** 
      - More so if the **observer object** can be **deleted**
- Therefore you must **provide** a *means* to **remove** the **object** from the **container** before you **completely delete** the *object*
  - Where there is a **shared_ptr** reference count.
- Easier if the code can **simply** just *add the pointer* to the **object** to the **container** in a way that **does not** *maintain* some **reference count**
  - but just allows you to **check** when the **pointer** is used if the **pointer is dangling** or points to **existing object**

---

- This is called a **weak pointer**
  - C++ provides the **weak_ptr** class
    - Can **not use** this object directly as there is **no dereference operator**
- You create a **weak_ptr** object *from* a **shared_ptr** object
  - When you want to **access** the *resource* $\to$ create a **shared_ptr** object from the **weak_ptr** object
    - Means that a **weak_ptr** object has the **same raw pointer** alongside *access* to the **same control block** as the *shared_ptr* object
    - Does **not take part** in the **reference counting**

---

- After creation $\to$ **weak_ptr** object enables you to *test* if the **wrapper pointer** is to **existing / destroyed** resource
  - There are **two methods**
    1. call the **member function** *expired*
    2. Attempt to **create** a **shared_ptr** from the **weak_ptr**
- Have collection of **weak_ptr** objects $\to$ may **iterate** over the collection
  - and **call expired** on each ne
    - If the method **returns true** $\to$ **remove the object** from collection
- As **weak_ptr** has *access to control block* created by **original** *shared_ptr*
  - This can **test** for the **zero reference**

---

- Second way to **test dangling** is creating a **shared_ptr** object from it
  - Two choices
    1. Create the **shared_ptr** by passing the **weak pointer** to *its constructor.*
       - If **expired** $to$ the constructor **throws** a **bad_weak_ptr** excepton
    2. Call the **lock** method on the **weak pointer** and if **expired** , **shared_ptr** object is **assigned to nullptr** then *test for this*

```c++
  shared_ptr<point> sp1 = make_shared<point>(1.0,1.0); 
    weak_ptr<point> wp(sp1); 

    // code that may call sp1.reset() or may not 

    if (!wp.expired())  { /* can use the resource */} 

    shared_ptr<point> sp2 = wp.lock(); 
    if (sp2 != nullptr) { /* can use the resource */} 

    try 
    { 
        shared_ptr<point> sp3(wp); 
        // use the pointer 
    } 
    catch(bad_weak_ptr& e) 
    { 
        // dangling weak pointer 
    }
```

- A **weak pointer** does *not alter reference count* 
  - can use it as a **back pointer** to **break cyclic dependency**
    - Often makes sense to use a **raw pointer** instead as a **child object** *cant exist* without the **parent object.**

## Templates in classes

- Classes **can be templated**
  - Can write **generic code** and **compiler** generates a *class* with those types
- The **parameter can be**
  1. tpyes
  2. constants
  3. integer values
  4. variadic versions (zero + parameters ,as given by code using the class)

```c++
 template <int N, typename T> class simple_array 
    { 
        T data[N]; 
    public: 
        const T* begin() const { return data; } 
        const T* end() const { return data + N; } 
        int size() const { return N; } 

        T& operator[](int idx)  
        { 
            if (idx < 0 || idx >= N) 
                throw range_error("Range 0 to " + to_string(N)); 
            return data[idx]; 
        }  
    };
```

- Simple **array class** defining the **basic iterator functions**
  - alongside **indexing operator**

```c++
  simple_array<4, int> four; 
    four[0] = 10; four[1] = 20; four[2] = 30; four[3] = 40; 
    for(int i : four) cout << i << " "; // 10 20 30 40 
    cout << endl; 
    four[4] = -99;            // throws a range_error exception
```

- Choose a **definition** out of th **class declaration**
  - Then you need **to give template** and its **parameters** as *part* of the **class name**

```c++
    template<int N, typename T> 
    T& simple_array<N,T>::operator[](int idx) 
    { 
        if (idx < 0 || idx >= N) 
            throw range_error("Range 0 to " + to_string(N)); 
        return data[idx]; 
    }
```

- Can have **default values** for **template parameters**

```c++
    template<int N, typename T=int> class simple_array 
    { 
        // same as before 
    };
```

- If you **think** you should have a **specific implementation** for a **template parameter**
  - Can provide the **code** for that version as a **specialization** of the template

```c++
  template<int N> class simple_array<N, char> 
    { 
        char data[N]; 
    public: 
        simple_array<N, char>(const char* str)  
        {  
            strncpy(data, str, N);  
        } 
        int size() const { return N; } 
        char& operator[](int idx) 
        { 
            if (idx < 0 || idx >= N) 
                throw range_error("Range 0 to " + to_string(N)); 
            return data[idx]; 
        } 
        operator const char*() const { return data; } 
    };
```

- With **a specialization**
  - You *do not get any code* from the **fully templated class**
    - You must **implement** all the methods to provide
      - Methods are **relevant** to the **specialization** but *not available* on **fully templated class**
- Example here is **partial specialization**
  - Specialized on a **single parameter** (T type of data)
- This class will be **used** to **declare variables** on the **type** `simple_array<n,char>`
  - where n is an integer.
    - Free to have **fully specialized template**
      - In this case $\to$ will be a **specialization** for a **fixed size** and a *specified type*

```c++
    template<> class simple_array<256, char> 
    { 
        char data[256]; 
    public: 
        // etc 
    };
```

- Possibly **not useful** in this case
  - The idea however is there for **special code** for *variables* that require **256 chars**

https://www.geeksforgeeks.org/template-specialization-c/

