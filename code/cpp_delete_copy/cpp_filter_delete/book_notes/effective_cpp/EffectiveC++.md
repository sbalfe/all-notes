# Effective C++

## Item 1 - View C++ as a federation of languages

> Rules for effective C++ programming vary, depending on the part of C++  we use.

- C++ is **multi paradigm** supports

  - Functional
  - OOP
  - Generic
  - Procedural 

- C++ is a *federation* composed of **sub languages**

  - C - shares deep logic composed of C.
  - OOP C++ 
  - Template C++
  - STL 

- STL $\to$ use pass by value references for C logic
- C++ OOP $\to$ use pass by reference as of move/copy constructors
- C $\to$ pass by value



## Item 2 - Prefer consts, enums, and inlines to #defines

> --- Key Points --
>
> For simple constants, prefer const objects or enums to #defines
>
> For function like macros , prefer inline functions to #defines
>
> Prefer `std::string` to `char*` 
>
> Make class constants `static` to limit their scope
>
> To access address of a class member denoted `static const p_member = 4` , define in an implementation file `const int ClassName::p_member`
>
> Can only initialise in the definition for **integral types** `const double ClassName::integral_member = 1.34` 
>
> Illegal to take the address of `enum`, use this as a constraint to prevent access to integral constants.
>
> Do not use macro functions , use inline `template` functions

- Prefer the *compiler* to the **pre-processor**  

```c++
#define ASPECT_RATIO 1.653 /* compiler may never see / removed / cause debug confusing things*/
```

- These **dont** get added to **symbol table** thus hard to debug.

```c++
const double AspectRatio = 1.652 // prefer a constant
```

- Blind macro name substitution could result in **copies**.

---

- Constant definitions often placed in **header files** for wide access
  - For *pointers* to to const thus declare the *pointer* const also.

```c++
const char* const authorName = "Scott Meyers";
```

- *Prefer* `std::string` to `char*` 

```c++
const std::string authorName {"Scott Meyers"};
```

- Make class constants **static** to **limit their scope**

```c++
class GamePlayer {
    private:
    	static const int NumTurns = 5; // constant declaration
    	int scores[NumTurns]; // use of constant
}
```

- This is a declaration and **not definition** as is for *integers / chars / bools*
- If you **take the address** you must declare a **definition** 

```c++
const int GamePlayer::NumTurns; // definition of NumTurns
```

- Placed in a **implementation file** and *not a header file* 
- **Initialisation** **not** **required** in implementation as this is done in the **definition**.
- macro names cannot be *encapsulated* to be private, they are global.

```c++
class CostEstimate {
    private:
    	static const double FudgeFactor; // declaration of static class
    									 // constant; goes in header file
};

const double CostEstimator::FudgeFactor = 1.35; // definition of static class, constant goes in impl file.
```

- *enum hack* $\to$ makes `NumTurns` a symbolic name for 5 for compilation where array size must be declared and be const

```c++
class GamePlayer {
    private:
    	enum {NumTurns = 5};
    	int scores[NumTurns];
}
```

- Cannot take the address of `enums` , use this to **constrain** access to any integral constants.
- enums never perform unnecessary memory allocation as such with `const` objects under some compilers.

---

- Bad to create *functions* in the macros, can cause unwanted side effects

```c++
#define CALL_WITH_MAX (a,b) f((a) > (b) ? (a) : (b))

int a = 5, b = 0
CALL_WITH_MAX(++a, b); // a is incremented twice
CALL_WITH_MAX(++a, b+10) // a is incremented once
```

- Get efficiency of macro using **inline function**, ensuring **type safety** of a *regular function*

```c++
/* dont know what T is , we pass reference to a const type - item 20*/
template <class T>
inline void callWithMax(const T& a, const T& b)
{
    f(a > b ? a : b)
}
```

- Obeys scope / access rules.
- Use `preprocessor` for controlling compilations and including files.

## Item 3 - Use Const whenever possible

> -- Key points --
>
>
```c++
> const char * p = "cunt"; // const char pointer
> const char * const p = "cunt"; // a const pointer that points ot a const char
> char const * const p = "cunt"; // same as line above
> char * const p = "cunt" // char pointer that is const.
> ```
> - User defined types can create odd expressions such as
>
> 
```c++
> BumClass a,b,c
> 
> (a*b)= c /* if BumClass overloads the `*` operator and makes it non const this is valid. */
> ```
> - Always declare *const* parameters unless it changes within the function 
> - Use const member functions to enable passing const references to member functions /  better client code
> - Const qualified member functions affects the member function in a class that is to be overloaded.
> - When overloading functions / functions in general, the return value cannot be assigned if its not returned as reference
>   - This would be like setting R value = R value, it must be L value = R value
> - C++ uses bitwise constness to compare actual bits a value has.
>   - Logical constness is indirect object changes such as from a reference/pointer.
> - `mutable` $\to$ declare an object that can be changed even for const objects
> - share functionality of a const/non const member functions by casting to const for both then removing that const value on return which should still be safe.

- Mark variable that its **invariant** to other programmers / compiler.
- Uses are *versatile*
  1. global / namespace
  2. objects declared static at file / function / block scope
  3. Inside classes
  4. static / non static data members
  5. Pointers, specify whether the pointer itself is const / data it points to / both / neither.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220225000007875.png)

- The const can be before or after the type but both before the asterisk and hold the same meaning

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220225000157491.png)

---

- Declaring *iterators* in STL const is similar to **declaring pointers const**.
  - Use a `const_iterator` implementing the idea of `const T* pointer`.

``` c++
std::vector<int> vec;

const std::vector<int>::iterator iter = vec.begin();
*iter = 10; // OK, changes the value it points to
++iter; // error iter is const

const std::vector<int>::const_iterator citer = vec.begin();
*cIter = 10; // error, constant iterator does not change values
++cIter; // okay iterator itself is not constant.
```

---

- Const for *functions declaration* can refer to many things

```c++
class Rational {...};

/* const return type often innapropriate , may reduce client errors with giving up safety /efficiency */
const Rational operator*(const Rational& lhs, const Rational& rhs);
```

- Require const `operator*` 

```c++
Rational a,b,c;


(a * b) =  c;

/* can be implicity converted to a boolean */
if (a * b = c){} // accidental comparison
  
```

### Const member Functions

- Make interface easier to understand and know which functions may modify an object
- Make it possible to write const objects.
- Performance Boost is passing objects by `reference-to-const` only possible with **const member functions**

---

- Member functions different in the **constness** can be **overloaded** (called via different modifier)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220225010731630.png)

- Const objects arise when passed by `reference-to-const` or passed by pointer

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220225010917755.png)

- overloading `operator[]` , giving different versions different return types, we can have `const` and `non-const` TextBlocks handled in different ways

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220225011121858.png)

- Error on return type of `operator[]` 
  -  Attempt to make an assignment to a `const char&` 
    - As this is what is returned from the `const` version of `operator[]`. 

- return type of **non const** `operator[]` is a *reference* to a `char` 
  - If it returned a *normal char* then this would break:

```c++
tb[0] = 'x';
```

- the `[]` is viewed as a function and if we return `char` not `char&` then it would complain as it expects an **addressable L value**.

---

### Physical vs Logical Constness

- `bitwise` $\to$ member function const if it **does not modify** the *objects data members* (*excluding static*).
  - Check for **physical constness** of the underlying data values and is used for C++
- Most *member functions* do not pass bitwise test.
  - Example in modifying what a pointer points to.

```c++
class CTextBlock {
    public:
    	char& operator[](
            std::size_t position
        ) const // inappropriate (but bitwise const) declaration of operator[]
        {
            return pText[position];
        }
    private:
    	char *pText;
};
```

- This is fine for the compiler as **nothing is actually changed**.

```c++
const CTextBlock cctb("Hello"); // declare constant object
char *pc = &cctb[0]; // call const operator[] to get a pointer to cctbs data.
*pc = 'J'; // cctb has value jello now
```

- This is clearly wrong is a **const objects** class attributes can be changed.

### Avoid duplication in const and non const member functions

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220225205835069.png)

- Find a way to do the same operations on these const / non const operations and use the **same return value** with **no double calls**
- Having a **non const** value call a *function* that is *const* is fine if we

```c++
/* 

	const version of operator[] = non version in operations, except for a const qualified return type

*/

/* 
    casting away the const on return is safe, as whoever called the non-const operator[] 
	must have already had a non const object otherwise this call was a mistake
*/

/*
	having non const operator[] call the const version is a safe way to avoid duplication,
	even if it requires a cast.
*/

class TextBlock {
    public:
    	const char& operator[](std::size_t position) const // same as before
        {
            return text[position];
        }
    
    	char& operator[](std::size_t position){
            /* both now call this function, both are casted to call the function above
               and then returns a non const qualfied function, this reduces code duplicaiton
               and prevents recursive calling of the const value
            */
            return const_cast<char&>(static_cast<const TextBlock&>(*this)[position]);
        }
};
```

## Item 4 - Make sure that objects are initialized before they are used

> -- key points --
>
> - No initialisation may be zero but they *may not*, essentially its undefined behaviour
> - Init all values 
> - All constructors must init everything in the object, **not an error** but not ideal.
>   - `=` in the constructor is assignment and not initialisation.
>   - init using the `:` initialisation list syntax as its **better performance** as = calls constructor each time then sets values in this non default constructor.
>   - Use for consistency the initialisation list 
> - `int` and shit are all objects thus have constructors in same sense recall.
> - `const` and `&` members **must be initialized**
> - When there are multiple constructors, defer some values to other constructor for initialisation.
>   - Called pseudo initialisation. 
> - Derived classes init before Base, data members init in order of declaration.
>   -  Init members in order they appear in the class
> - `static` objects are defined and never go throughout the programs lifetime.
> - order of init of non local static objects defined in different translations units is undefined.
>   - This mean the non local static object *could be unitialized*. 
>   - example being an object you need to use in some other file may not have been created and you try to access its methods before its been created but as its undefined there is no sure way its gonna work.
>   - Create some function that has local static objects and return by reference to fix this issue.
>     - manually invoke these for multithreading to ensure they are init.

```c++
/* p and x may have zero values but not ensured. */
int x;
class Point {
    int x,y;
}
Point p; 
```

- To get around this idea just **always** *init the values*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220225235704592.png)

- This is the solution to **non local static objects.**

```c++
/* file.h*/

class FileSystem {
public:
	std::size_t numDisks() const;
};
extern FileSystem tfs; /* declared somewhere else*/
```

```c++
/* main.cpp */
#include "file.h"
class Directory {
  public:
  Director() {
    std::size_t disks = tfs.numDisks(); /* error as tfs may not be initialised*/
  }
};
Directory tempDir();
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324222727387.png)

- Solve using a reference returning function

```c++
class FileSystem {
public:
	std::size_t numDisks() const;
};

inline FileSystem& tfs() {
	static FileSystem fs;
	return fs;
}
```

```c++
#include <iostream>
#include "file.h"


class Directory {
public:
	explicit Directory(int);
};

Directory::Directory(int) {
	std::size_t disks = tfs().numDisks(); // access safely now 
}

int main() {
	Directory(1);
	return 0;
}
```



## Item 5 - Know what functions C++ silently writes and calls

> -- key points --
>
> - Constructors are created by compiler 
>   - Copy / move / default constructor / destructor (non virtual if not derived from virtual base class)
>   - Default not created if you define other constructors.
> - To support copy construction for referenced data members we must implement copy constructors ourselves.
> - Compilers reject const copy constructors for const data members
> - Derived classes taking base class private copy constructors are rejected too.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220226002026550.png)

```c++
std::string newDog("Pers");
std::string oldDog("Sat");

NamedObject<int> p(newDog, 2);
NamedObject<int> s(oldDog, 36);

/* should p.nameValue now refer to s.nameValue i.e. change it reference something else.
	vice versa should p's reference to nameValue be replaced with s.nameValue
	
	DOES NOT COMPILE
*/
p = s
```

## Item 6 Explicitly disallow the use of compiler generated functions you dont want

> -- key points --
>
> - prevent copy by removing the ability `=delete` 
> - old method:
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220226005912697.png)

## Item 7 - Declare destructors virtual in polymorphic base classes

> -- key points --
>
> - Factory functions that define a base class pointer to derived objects need a virtual destructor to call the destructor.
>   - Results are undefined when a base class pointing to a derived class on heap is deleted, given the destructor of the base class is non virtual.
>   - Leaks resources when non virtual destructors break only the base part of the object.
> - Virtual is polymorphic classes basically thus base pointing to derived calling a virtual function with same name on both , calls the derived one if virtual is used.
> - If a class is not a base class then dont use virtual destructors.
> - Virtual table pointers are used to link the function pointers correctly.
>   - These actually increase the size of the objects as the virtual pointer table.
>     - https://stackoverflow.com/questions/1626290/c-virtual-function-table-memory-cost
>   - This is a drawback of virtual table pointers.
> - abstract base classes must have abstract virtual destructors which are also defined.
>   - Compiler generates calls to this from derived class destructors, thus each destructor must be defined in the derived classes.
> - destructors called in order from most derived > base.

---

```c++
class SpecialString: public std::string {} // std::string has a non virtual destructor!! 
```

- Thus trying to do this

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220226013507534.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220226034213257.png)

## Item 8 - Prevent exceptions from leaving destructors

> -- key points --
>
> - Throwing exceptions in destructors is dangerous, never let them leave the destructors
> - destructor exceptions can propagate and build up in C++ which is bad and leads to undefined behaviour.

- Consider some class to handle a database connection

```c++
class DBConnection
{
public:
    ...
    static DBConnection create();
    void close();   // it will throw an exception
};
```

- Have some class to handle its resources. 
  - Ensure clients dont forget to call *close* there is some **resource managing class**

```c++
class DBConn
{
public:
    ...
    ~DBConn()
    {
        db.close();
    }
private:
    DBConnection db;
};
```

- Exception can propagate out of the destructor which
  - Solution would be to provide some *close method* and *catch* within the destructor swallowing the exception if clients dont call close method themselves.

```c++
class DBConn
{
public:
    ...
    void close()
    {
        db.close();
        closed = true;
    }
    ~DBConn()
    {
        if (!closed)
        {
            db.close();
        }
        catch(...)
        {
            // log that close failed and swallow the exception
        }
    }
private:
    DBConnection db;
    bool closed;
};
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227163802343.png)

## Item 9 - Never call virtual functions during construction or destruction

> -- key points -- 
>
> - Calls never go to more derived class when calling virtual functions during construction / destruction
>   - Essentially if you call a virtual function in the transaction constructor it calls the base class functions implementation and not the one you expect essentially.
>   - This is because derived classes may **not be initialized**.
> - `dynamic_cast` treats the object as base class also when virtual functions are called in constructor.
> - stop calling fucking virtuals in base class constructors you retards.
> - Same idea for **destructors**.
> - Compensate by having derived classes constructors call construction information up to base class constructors.



![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227172236841.png)

- Compiler would not see `logTransaction()` implementation for pure virtual function.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227172557998.png)

- Make the constructor call the private member function in base class that calls a virtual function itself.
  - The idea is same as issue before but compiles and links okay this time
  - logTransaction is *pure virtual function* thus most should abort, non pure it calls the transaction implementation of course.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227173219306.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227174621519.png)

- Make `createLogString` function `static` to ensure we dont accidentally call buy transactions members which are still under construction
- Use this helper function to make it more readable when passing parameter up to derived.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227175536766.png)

## Item 10 - Have assignment operators return to a reference of $\ast this$ 

> -- key points -- 
>
> - copy assignment returns reference to this

```c++
#include <iostream>
class Widget {
public:

	Widget& operator=(const Widget& rhs) { /* return type is a reference to current class*/
		*this = rhs;
		return *this; /* return left hand object */
	}
};

int main() {
	int x, y, z;
	x = y = z = 15;

	/* 15 assigned to z, this returns a reference to the LHS argument*/
	x = (y = (z = 15));
}
```

- Applies convention to all assignment operators, not just standard form above.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227185308061.png)

- This is **a convention** and thus works **if not**.

## Item 11 - Handle assignment to self in $\text{operator=}$

> -- key points -- 
>
> - self assignment is assigning some object to itself.
>
> - **aliasing** $\to$ having more than one reference to the same object
>
>   - Does not have to be the same type
>     - A base class ref/pointer can refer/point to object of a derived class type.
>
> - use identity test at top of *operator=* to test for self assignment, consider a balance of exception / self assignment on program use case, if statements incur branch preventing pipeline flow.
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227201931453.png)
>
> - Exception safety in copy assignment = self assignment safety.
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227202757506.png)

```c++
a[i] = a[j]; /* potential self assignment */
*px = *py; /* example of aliasing */
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227201419453.png)

- Chance the base and derived are the exact same object.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227201713118.png)

- $*this$ and $rhs$ could be the same object
  - This case means `delete pb` would delete the `rhs` too therefore pointing to a deleted objects.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227202010039.png)

- This fixes but now still not exception safe.
- It points to deleted bitmap if there is not enough memory for example.
- Achieve exception safety to achieve self assignment safety.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227202747726.png)

- May not work most efficiently but its a solution.
- If bitmap throws some exception $\to$ pb (and widget its within) are unchanged.
  - Even with no identity test, the code handles assignment to self.
    - As we make a copy of the original bitmap, point to the copy we made then delete the original bitmap.

---

- Apply copy and swap technique

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227204200443.png)

- Variation in this takes advantages of the facts that
  - A class copy assignment operator may be declared to take its argument by value
  - Passing something by values make a copy of it.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227204802917.png)

- This may generate more efficient code.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227204831883.png)

## Item 12 - Copy all parts of an object

> -- Key points --
>
> - Only copy constructor and copy assignment can copy items.
> - Ensure copy new data members as compilers dont warn about it.
> - Issue arises mostly in inheritance when you dont consider the classes above.
> - Never call copy constructor in copy assignment method, vice versa.
> - To reduce duplication use `init` in copy assignment / copy constructor.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227213439273.png)

- Copy the base class customer by passing values from derived to base.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220227214752856.png)

## Item 13 - Use Objects to manage resources

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324192715496.png)

- use smart pointers.

## Item 14 - Think carefully about copying behaviour in resource managing classes

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324192944871.png)

> 1. Must **prohibit copying**
>    - Copying a **RAII** class makes sense so *make the constructor private*
> 2. **Reference count** the *underlying resource*
>    - Use **shared pointer** as a member of RAII class to enforce this.
>    - Use a **custom deleter** to *to control behaviour* of *reference count at 0* effect.
> 3. **Copy the underlying resource**
>    - Perform **deep copy** of the *resource object.* to obtain all its objects.
> 4. **Transfer ownership** of the **underlying resource** 
>    - Keep a single object referencing some raw resource always therefore transfer ownership
>      - Does with smart pointers.

- Only code **guaranteed** to run after **exceptions** are *destructors* of objects **residing on the stack**.

- Resource **management** $\to$ linked to the **lifespan** of *suitable objects* to gain **automatic allocation** and **reclamation**.

  

## Item 15 - Provide access to raw resource in resource managing class

```c++
// You have a shared pointer
std::shared_ptr<Foo> foo(createFoo());

// But the API only accept Foo
void acceptFoo(Foo * foo);
```

- This must make the RAII resource **return raw object**
- `shared_ptr` has `get()` for this. Also *overload* `->` and `*`. 
- Alternatively use **implicit conversion** by `overloading` `handle` used to manage the resource

```c++
class Font
{
public:
    void changeFontSize(FontHandle f, int newSize);
    explicit Font(FontHandle fh):
    f(fh)
    {}

    ~Font(){releaseFont(f);}

    // explicitly convert Font to FontHandle
    operator FontHandle() const
    {return f;}
    ....
private:
    FontHandle f;
};
```

```c++
Font f1(getFont());
...
// oops! Intent to copy a Font object, but instead implicitly
// converted f1 to FontHandle and then copied that.
FontHandle f2 = f1;

/*Now the program has a FontHandle being managed by the Font object
f1, but the FontHandle is also available for direct use as f2. That’s
almost never good. For example, when f1 is destroyed, the font will be
released, and f2 will dangle.*/

changeFontSize(f, newFontSize); // this would implicit convert Font to Font handle without get operator.
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324205009674.png)

- Explicit is safer as it prevents accidental conversions you were not aware of.
- Essentially prefer using safe `get` operators as implicit conversions are not as safe.

## Item 16 - Use the same form in corresponding uses of $new$ and $delete$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324210820905.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324210909160.png)

- Without using `delete[]` , a `delete` just refers to the **first item** in the array.
- It must know **how many objects** reside in the memory location that are **being deleted**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324211304907.png)

- its **undefined** using `[]` on a `new` with no array, it would invoke destructors on invalid memory potentially.

## Item 17 - Stored $new$ed objects in smart pointers in standalone pointers

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324211851231.png)

```c++
processWidget(std::share_ptr<Widget> {new Widget}, priority());
```

- There is **no implicit conversion** from *raw pointer* to `shared_ptr`

```c++
processWidget(new Widget, priority());
```

- Chance that *priority()* called *before* the widget construction `std::shared_ptr<T>`  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324212234518.png)

- We run into a **leak** if *priority()* somehow causes an **exception to occur**.
- Therefore use a **seperate statement** to perform the **shared pointer** *initialisation*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324212334671.png)

## Item 18 - Make interfaces easy to use correctly and hard to use incorrectly.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324212559628.png)

> - Make the interface cause a **compiler error** if *not used as intended*.
>
> - Make member functions use `const` if they do **not change values** inside them.
>
> - Follow primitive type laws regarding whats **legal** such as with shit like this where overloaded * is non const return
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324213718627.png)
>
> - Interfaces that expect a user to **remember something** are *bad* and prone to incorrect use.
>
>   - Example 
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324213922782.png)
>
>   - Use a `std::shared_ptr`  for `createInvestment()` as this manages the *delete cycle*.
>
>   - Ideally it **returns** this `std::shared_ptr<Investment>` 
>
>   - Best case return a *deleter* if the pointer must be used to **pass into something else**
>
>   - ```c++
>     std::tr1::shared_ptr<Investment> // attempt to create a null
>     pInv(0, getRidOfInvestment); // shared_ptr with a custom deleter;
>     // this won’t compile
>     ```
>
>   - This $0$ is invalid and therefore enforce a static cast thus creating the return type as 
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324214659971.png)
>
> - Cross DLL problem 
>
>   - `new` in one DLL
>   - `delete` in seperate DLL
>   - Leads to **runtime errors**.
>   - This is avoided by using `std::shared_ptr` from which has a **default deleter** from the same DLL.
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324215007967.png)
>
>   - Pass this pointer amongst DLLs and deletes properly when reference counter becomes 0.

```c++
// Class date which is easy to use and not easy to use wrong.
class Date
{
public:
    struct Day
    {
        explicit Day(int d):val(d){}
        int val;
    };
    struct Month
    {
        explicit Month(int m):val(m){}
        int val;
    };
    struct Year
    {
        explicit Year(int y):val(y){}
        int val;
    };

    Date(const Month& m, const Day& d, const Year& y)
    : m_month(m)
    , m_day(d)
    , m_year(y) {};
    ~Date(){};

private:
    Month m_month;
    Day   m_day;
    Year  m_year;
};

Date d1(Date::Month(3), Date::Day(3), Date::Year(2000)); // Ok

// Compilation error!
// no matching function for call to 'Date::Date(Date::Day, Date::Month, Date::Year)'
Date d2(Date::Day(3), Date::Month(3), Date::Year(2000));
```

- Use this data function rather than just a simple constructor taking **3 integers** as its not clear which of these refer to month / day/ year as its **regional**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324213143646.png)

- This defines the **valid months clearly** so there are no **type errors**.

## Item 19 - Treat class design as Type design

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220324215244073.png)

> - Various configurations , overloads, allocations that can be done thus carefully consider each option.
> - Define how to **create** and **destroy** objects of our class.
>   - Concerned with *constructors* and *destructors* 
>   - Calling `new` and `delete` if required.
> - Difference between **object initialization** and **object assignment** 
>   - Refer to *item 4*
> - What does it mean for **objects** of your **new type** to be **passed by value**
>   - The *copy constructor* defines how **pass by value** is *implemented*.
> - Restrictions on the **legal values** for the **new type**
>   - Only set combinations of **class members** are valid
>   - These combinations *determine the invariant* the *class maintains*.
>   - These invariants determine **error checking** within *member functions*
>   - Also in constructors and get/set.
>   - Affects the **exceptions** the functions throw.
> - Does the **new type** fit into an ***inheritance graph***.
>   - Inheritance from a class **constrains** us to that classes design.
>   - Important when the functions are **virtual** or **non virtual**.
> - What are the **type conversions** allowed on this **new type**.
>   - Wishing `t1` to be *implicitly converted* into objects of type `t2` $\to$ write **type conversions** or **non explicit** *constructor* in class `t2` called with **one argument**
>   - Enable explicit by **writing functions** for the *conversion*, avoid making them **type conversion operators** or *non explicit constructors* called with **one argument**
> - What **operators** and **functions** make *sense* for our new type
>   - Functions in question , some are members , some not
> - What **standard functions are disallowed**
>   - Declare `= delete`
> - Who should access the **members** of the **new type**
>   - Public
>   - Protected
>   - Private
>   - Helps determine the *classes* and/or functions that are **friends**.
>   - Whether **class nesting** makes sense
> - What is the **undeclared interface** of the **new type?**
>   - guarantees with respect to the **performance**
>   - Exception safety
>   - Resource usage (locks / dynamic memory allocation)
>   - Guarantees you offer in these areas impose **constraints** on the *class implementation*
> - How general is the **new type**? 
>   - We may **not be defining a new type**
>   - Maybe some **family of types**
>   - We must make a *class template* in this case.
> - Is a **new type required**
>   - If you define a **new derived** class only so we **add functionality** to some *existing class*
>   - We are better of defining one **or more member functions** or **templates**.

## Item 20 - Prefer pass by reference to const to pass by value

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220325011410298.png)

- As a default C++ , passes **objects** to and from functions by **value**
- Function parameters $\to$ initialise with **copies** of actual arguments.
- Function *callers* obtain a **copy** of value *returned by function* provided by the **objects copy constructor**
  - It calls the **destructor** of course also when its deleted in some contexts.
- This is therefore an **expensive operation**.
- Ensure `const` as a *value* already implies this but not clear for **reference**.

- Passing by **value** as a *derived class* will **split** the *derived properties* this is **slicing**, reference prevent this.
- References in sense are implemented as **pointers**
- *built in types* pass by **value** is *not unreasonable*.
- STL iterator and function object types **pass by value**.
- STL containers are **small pointers sometimes** but *copying* what they point to **can be costly**. 
- Objects consisting of **just doubles** may not **place into a register** , opposed to **naked doubles**, thus **pass by reference** as *pointers* go in *registers always*.

## Item 21 - Dont try to return a reference when you must return an object.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220325184045268.png)

- Try to **use pass by value** for *everything*
- Passing **references** to *objects that dont exist* is a **bad thinig**

```c++
class Rational
{
public:
	Rational (int numerator = 0, int denominator = 1);
	...
private:
	int n, d;

	friend const Rational& operator*(const Rational& lhs, const Rational& rhs)
	{
		Rational result(lhs.n * rhs.n, lhs.d * rhs.d);
		return result;
	}
};
```

- Rational result returned destroyed thus **object does not exist anymore**
- Same rule applies to a **pointer** to some **local stack object**, a 

## Item 22 - Declare data members private

- Seems **obvious** to *declare members private*.
- Sometimes **you may violate** this **without realizing it**.
- Defining **data members** private *is all about encapsulation*.

---

-  Hiding **data members** from the **clients**, can *ensure* that class **invariants** are *always maintained*
- Reserve the **right** to *change* the **implementations** decisions later.
- Not hiding $\to$ find that even if **we own the source code** $\to$ the ability to *change* anything public is **extremely restricted**, as *too much client code* is **broken**
- Public $\to$ means **==encapsulated==**, and *practically* speaking **unencapsulated** means **unchangeable** , especially for *classes* that are **widely used**.
-  Generally speaking, we should **restrict** as much as **possible** for *accessing data members*
- Fewer public data members, better the **encapsulate it**

## Item 23 - Prefer non member non friend functions to member functions

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220325190745049.png)

- Imagine you have following class

```c++
class WebBrowser {
public:
  ...
  void clearCache();
  void clearHistory();
  void removeCookies();
  ...
};
```

- We want to **add** another *function* `clearEverything` which calls `clearCache` , `clearHistory` and `removeCookies` which way is better.

```c++
// first approach is add clearEverything as member function
class WebBrowser {
public:
  ...
  void clearEverything() {
    clearCache();
    clearHistory();
    removeCookies();
  }
  ...
};

// second approach is add non-member function
void clearBrowser(WebBrowser& wb)
{
  wb.clearCache();
  wb.clearHistory();
  wb.removeCookies();
}
```

- Second approach provides **better encapsulation**.
- The *more public methods* which access *private data* $\to$ the **worst encapsulation** in some class.
- Use *existing method* of `WebBrowser` without **add public interface to it**, yielding **better encapsulation**.

---

- Above *example* $\to$ `clearBrowser` is a **utility function** the *client can call*.
- Defining it **in a namespace** related to `WebBrowser` class is **nature way** of *providing the functionality* 
- Having other **utility** function which may **not be needed** by *all clients*.
- Using **namespaces** provides better of **isolation** of *functionalities* 
- This is *how* STL *organize* its **classes and functionalities**, rather than **monolithic** standard library header containing `std namespace` , there is `vector` , `algoritihm`. 

```c++
// header "webbrowser.h" — header for class WebBrowser itself
// as well as "core" WebBrowser-related functionality
namespace WebBrowserStuff {
   class WebBrowser { ... };
   void clearBrowser(WebBrowser& wb) { ... };
}
// header "webbrowserbookmarks.h"
namespace WebBrowserStuff {
   void addBookmark(WebBrowser& wb) { ... };
}
// header "webbrowsercookies.h"
namespace WebBrowserStuff {
   void validateCookies(const WebBrowser& wb) { ... };
}
```

## Item 24 - Declare non member functions when type conversions should apply to all parameters

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220325192601147.png)

- Define a `Rational` class to **represent** the *rational numbers* as such

```c++
class Rational {
public:
  Rational(int numerator = 0,
           int denominator = 1);
  int numerator() const;
  int denominator() const;
private:
  int m_numerator;
  int m_denominator;
};
```

- And you wish to **support rational number arithmetic** like *multiplication*
  - Instinctive tells you to **implement** as a member functions

```c++
class Rational {
public:
...
const Rational operator*(const Rational& rhs) const;
};
```

- Seems fine but when actually **using**

```c++
Rational oneEighth(1, 8);
Rational oneHalf(1, 2);
Rational result = oneHalf * oneEighth; // fine
result = result * oneEighth;           // fine
result = oneHalf * 2;                  // fine
result = 2 * oneHalf;                  // error!
```

- Last operation does **not compile** as its saying `result = 2.operator*(oneHalf)` which is obviously an error
- `2` has no `operator*`. 
- Define the *constructor* as **explicit**, we realize `result = oneHalf *` as **type conversion** implication of `2` not applied.

---

- Define `operator*` as a **non member function** which will *allow* compile to **perform implicit** *type conversion* for **all arguments** 

```c++
class Rational {
public:
  Rational(int numerator = 0,
           int denominator = 1);
  int numerator() const;
  int denominator() const;
private:
  int m_numerator;
  int m_denominator;
};
const Rational operator*(const Rational& lhs,
                         const Rational& rhs)
{
  return Rational(lhs.numerator() * rhs.numerator(),
                  lhs.denominator() * rhs.denominator());
}
```

- Therefore all **usage above** will *compile* and *work correctly*

## Item 25 - Consider support for a non throwing $swap$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220325203342502.png)

- How to **write a swap function** for *our own class*.
- This is *especially* useful when **default swap behaviour** is **too heavy for your class**.
- You can *imagine* this is how `std` implementation.

```c++
namespace std {
  template<typename T>
  void swap(T& a, T& b)
  {
    T temp(a);
    a = b;
    b = temp;
  }
}
```

- Classes implement the `pimpl` idiom like such, item 31.

```c++
class WidgetImpl {
public:
  ...
private:
  int a, b, c;
  std::vector<double> v;
  ...
};

class Widget {
public:
  Widget(const Widget& rhs);
  Widget& operator=(const Widget& rhs)
  {
   ...
   *pImpl = *(rhs.pImpl);
   ...
  }
  ...

private:
  WidgetImpl *pImpl;
};
```

- Swapping just requires **switching** the *implementation pointer* instead of the **entire object**.
- Thats when **you need to write** your own `swap`, most likely `swap` needs to **access private members**, so must be a **member swap method**.

```c++
class Widget {
public:
  ...
  void swap(Widget& other)
  {
    using std::swap;
    swap(pImpl, other.pImpl);
  }
  ...
};
```

- And if you **want to support** a *conventional* `std::swap` function.
- We can add some **specialized version** for `std::swap` which calls the *member* `swap` as such.

```c++
namespace std {
  template<>
  void swap<Widget>(Widget& a,
                    Widget& b)
  {
    a.swap(b);
  }
}
```

- May *wonder* why `using std::swap` in *above* member `swap` 
- `pImpl` is a **pointer type** is **pointer** and `std::swap` supports this.
- Could use `std::swap` directly, but above way **of calling** `swap` works for **all cases**, that dont include just pointers.

---

- It is when **user defined class** is a *template class*.
- Making `Widget` and `WidgetImpl` class templates

```c++
template<typename T>
class WidgetImplTP {
private:
    std::vector<T> v;
};

template<typename T>
class WidgetTP {
public:
    WidgetTP &operator=(const WidgetTP &rhs) {
        *pImpl = *(rhs.pImpl);
        return *this;
    }

    void swap(WidgetTP &other) {
        std::cout << "WidgetTP::swap\n";
        using std::swap;
        swap(pImpl, other.pImpl);
    }

private:
    WidgetImplTP<T> *pImpl;
};
```

- Naturally **provide** the *following specialized* `std::swap` implementation

```c++
namespace std {
    template<typename T>
    void swap<WidgetTP<T> >(WidgetTP<T> &a, WidgetTP<T> &b) {
        a.swap(b);
    }
}
```

- This will not compile as **function template error**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220325202801726.png)

- Use **overloading** instead of **total template specialization**
- There is *issue* doing so within `std` namespace.
- Okay to **totally specialize templates** in`std` but its **not okay to add new template** (classes / functions), leads to **undefined behaviour**

---

- Create a **namespace** and define a *free* `swap` there

```c++
namespace WidgetStuff {
    template<typename T>
    class WidgetImplTP {
    private:
        std::vector<T> v;
    };

    template<typename T>
    class WidgetTP {
    public:
        WidgetTP &operator=(const WidgetTP &rhs) {
            *pImpl = *(rhs.pImpl);
            return *this;
        }

        void swap(WidgetTP &other) {
            std::cout << "WidgetTP::swap\n";
            using std::swap;
            swap(pImpl, other.pImpl);
        }

    private:
        WidgetImplTP<T> *pImpl;
    };

    template<typename T>
    void swap(WidgetTP<T> &a, WidgetTP<T> &b) {
        std::cout << "WidgetTP::swap(WidgetTP<T>& a, WidgetTP<T>& b)\n";
        a.swap(b);
    }
}
```

- Any *user* of `swap` who may or **may not know** whether the **class implements** its own `swap` 
- Always brings `std::swap` into the scope by doing `using std::swap` then call `swap`.
- This way it will **call custom** `swap` function if **available**, otherwise `std::swap`.

```c++
template <typename T>
void doSomething(T& obj1, T& obj2)
{
  using std::swap;           // make std::swap available in this function
  swap(obj1, obj2);          // call the best swap for objects of type T
}
```

- The default `swap` offers **acceptable** *efficiency* for the class or class template.
- Anyone trying to `swap` objects of your **type** will get **the default version** and *that will work fine*

---

- If the *default implementation* `swap` is **not efficient enough** (class  / template using pimpl idiom)
  1. Offer a *public* `swap` member function that **efficiently** swaps the *value* of two objects of your type
     - This should never throw an exception
  2. Offer a non member `swap` in the **same namespace** as the **class** or **template**
     - Have it *call* you **swap member function**
  3. If we are *writing* a class (*not a class template*), *specialize* `std::swap` for our class. Have it call the **swap member function**.

## Item 26 - Postpone Variables definitions as long as possible

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220326161337927.png)

- The *rule* is very **obvious**, instead of

```c++
std::string encryptPassword(const std::string& password)
{
  using namespace std;
  string encrypted;
  if (password.length() < MinimumPasswordLength) {
      throw logic_error("Password is too short");
  }
  ...                        // do whatever is necessary to place an
                             // encrypted version of password in encrypted
  return encrypted;
}
```

- Define `encrypted` right before we **need to use it**

```c++
std::string encryptPassword(const std::string& password)
{
  using namespace std;
  if (password.length() < MinimumPasswordLength) {
     throw logic_error("Password is too short");
  }
  string encrypted;
  ...                      // do whatever is necessary to place an
                           // encrypted version of password in encrypted
  return encrypted;
}
```

- Another **common question** is *which way* is better looking at **following examples** of *doing* `for` loop.

```c++
// example 1
Widget w;
for (int i = 0; i < n; ++i){
  w = some value dependent on i;
  ...
}
// example 2
for (int i = 0; i < n; ++i) {
    Widget w(some value dependent on i);
    ...
}
```

- Consider whether to *execute* between **1 constructor + 1 destructor + n assignments**

- Alternatively **n constructors + n destructors** for example 2

## Item 27 - Casting

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220326164704359.png)

- There are **usually** *three* different ways to **write the same cast**.

```c++
(T)expression
// or
T(expression)
```

- There is **no difference** in *meaning* between these forms 
  - This is a **matter** of *where* you put the **parentheses**
    - These two **forms** are *old style casts*.
- C++ $\to$ offers us **four new cast forms**

```c++
const_cast<T>(expression); // cast away the constness of objects
dynamic_cast<T>(expression); // safe downcasting, determine whether an object is of particular type in inheritance hierarchy. It is only cast that cannot be performed using the old style syntax, runtime cost incurs.
reinterpret_cast<T>(expression); // low level casts that yield implementation dependent (unportable), results that are rare outside of low level code.
    
static_cast<T>(expression); // force implicit conversions (non const to const object, int to double)
// used to perform reverse of many such conversions
// such as void* pointers to typed pointers, pointer to base, pointer to derived
// cannot cast const to non const.
```

- Rule is to **avoid using cast** as *much as possible*
- `const_cast` is *always* a bad idea as it **breaks** the `const` semantics.
- `dynamic_cast` is a **indication** of *bad inheritance design* or *misuse of inheritance*.
- The **following code** should *never happen*.

```c++
class Window { ... };
...                                     // derived classes are defined here
typedef std::vector<std::tr1::shared_ptr<Window> > VPW;
VPW winPtrs;
for (VPW::iterator iter = winPtrs.begin(); iter != winPtrs.end(); ++iter)
{
  if (SpecialWindow1 *psw1 =
       dynamic_cast<SpecialWindow1*>(iter->get())) { ... }
  else if (SpecialWindow2 *psw2 =
            dynamic_cast<SpecialWindow2*>(iter->get())) { ... }
  else if (SpecialWindow3 *psw3 =
            dynamic_cast<SpecialWindow3*>(iter->get())) { ... }
  ...
}
```

- `static_cast` is *sometimes ok*, has a *caveat* as it creates a **new temporary**
  - Considering this code

```c++
class Window {
public:
  virtual void onResize() { ... }
  ...
};
class SpecialWindow: public Window {
public:
  virtual void onResize() {
    static_cast<Window>(*this).onResize();
    ...
  }
};
```

- Expect `static_cast<Window>(*this).onResize()` $\to$ equivalent to `Window::onResize()` , but its **not**.
- `static_cast<Window>(*this)` create a **temporary object** and `onResize()` is **invoked** on that *temporary object* instead of `this` object

## Item 28 - Avoid returning handles to Object internals

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220326170949647.png)

- Presume we have the **following classes**

```c++
class Point {
public:
    Point(): m_x(0), m_y(0){}
    Point(int x, int y): m_x(x), m_y(y) {}
    
    void setX(int newVal)
    {
        m_x = newVal;
    }
    void setY(int newVal)
    {
        m_y = newVal;
   	}
    std::ostream& print(std::ostream& out) const
    {
        out << "x: " << m_x << ", y: " << m_y;
        return out;
    }

private:
    int m_x;
    int m_y;
};

std::ostream& operator<<(std::ostream& out, const Point& val)
{
    val.print(out);
    return out;
}

struct RectData {
    Point ulhc;
    Point lrhc;
};

class Rectangle {
public:
    Rectangle(const Point& coord1, const Point& coord2)
        : m_data(new RectData())
    {
        m_data->ulhc = coord1;
        m_data->lrhc = coord2;
    }
    Point& upperLeft() const { return m_data->ulhc; }
    Point& lowerRight() const { return m_data->lrhc; }
private:
    std::shared_ptr<RectData> m_data;
};
```

- Issue is that `Rectangle` object returns a **handle** of the *internal* `Point`. 
- The methods are declared `const` however this handle means **they can be changed**.

```c++
int main() {
    Point a(0, 2);
    Point b(1, 5);
    Rectangle rec(a, b);
    std::cout << a << std::endl;
    std::cout << b << std::endl;
    std::cout << rec.upperLeft() << std::endl;
    std::cout << rec.lowerRight() << std::endl;
    rec.upperLeft().setX(99);
    rec.lowerRight().setY(-10);
    std::cout << rec.upperLeft() << std::endl;
    std::cout << rec.lowerRight() << std::endl;
    return 0;
}
```

- Prints

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220326170659898.png)

- Another example is if you **return** an **internal pointer** to the *caller*
  - May *outlive* the **life of your object** and the pointer becomes a **dangling pointer**
- Rule is to **avoid returning handles** (*references, pointers, iterators*) to **object internals**.
  - This *increases **encapsulation***
  - This *helps* the `const` member functions act `const` 
  - *Minimizes* the **creation** of **dangling handles**

## Item 29 - Strive for exception safety within code.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220328190039464.png)

- Class *representing* *GUIs* for **menus** with **background images**
- The *class* is *designed* to be used **in a threaded environment**, therefore contains a *mutex* for **concurrency control**.

```c++
class PrettyMenu {
public:
    ...
    void changeBackground(std::istream& imgSrc);
    ...
private:
    Mutex mutex;
    Image *bgImage;
    int imageChanges;
};

void PrettyMenu::changeBackground(std::istream& imgSrc)
{
    lock(&mutex);
    delete bgImage;                    // get rid of old background
    ++imageChanges;                    // update image change count
    bgImage = new Image(imgSrc);       // install new background
    unlock(&mutex);                    // release mutex
}
```

- Exception thrown in **exception safe functions**
  1. *No resources* are *leaked*. Code above **clearly fails** as if `new Image(imgSrc)`, expression **yields** an *exception*.
     - The *call to unlock* never gets **executed**, and the **mutex** is *held forever*.
     - Do *not allow* any **data structures** to become *corrupted*
       - Above if `new` above throws $\to$ `bgImage` left pointing to a **deleted object**
     - Image changes has been incremented, even though **its not true**.
- Always offer one of the **guarantees**

  1. Exception thrown $\to$ everything in the program stays **in valid state**.
     - No *corrupt* data structures/become corrupted, with a **consistent internal state** (*class invariants maintained*)
     - The *state* can be **unpredictable** 
       - Example above, choose to **not change image** or **select default**, clients **cannot predict this**.
  2. State of **program unchanged** after *exception thrown*.
     - *atomic calls* $\to$ if they succeed $\to$ they **succeed entirely**
       - Failure $\to$ was as if they were **never called** in the *first*.
  3. Functions **offering nothrow guarantee** promise *never* to *throw* **exceptions**, as they *always* do what **they promise to do**
     - All **operations** on *built in types* (*ints / pointers*) are *nothrow* (offer the **nothrow guarantee**)
       - This is a *critical building block* of **exception safe code**.

----

- Work with *functions* offering the **strong guarantee** is easier than some **basic guarantee**.
  -  A **strong guarantee** $\to$ leads to **2 program states** 
    1. As expected *following successful execution* of the *function.*
    2. State existed **before the function was called**.

- Contrast a **basic guarantee** yields an **exception**, program could be **in any valid state**.

---

- Write `changeBackground` as following

```c++
class PrettyMenu {
  ...
  std::shared_ptr<Image> bgImage;
  ...
};

void PrettyMenu::changeBackground(std::istream& imgSrc)
{
  Lock ml(&mutex);

  bgImage.reset(new Image(imgSrc));  // replace bgImage's internal
                                     // pointer with the result of the
                                     // "new Image" expression
  ++imageChanges;
}
```

- `changeBackground` offers **exception safe strong  guarantee** assuming `new Image(imgsrc)` wont *modify* the *state* of `imgSrc`. 

---

- **copy and swap** strategy is used
  - Helpful when you have **many states** that need to be *managed* or *restored* when **exception is thrown**
    - Combines well with **pimpl idiom.**


```c++
struct PMImpl {                               // PMImpl = "PrettyMenu
  std::shared_ptr<Image> bgImage;        // Impl."; see below for
  int imageChanges;                           // why it's a struct
};
class PrettyMenu {
  ...
private:
  Mutex mutex;
  std::shared_ptr<PMImpl> pImpl;
};
void PrettyMenu::changeBackground(std::istream& imgSrc)
{
  using std::swap;
  Lock ml(&mutex);
  std::shared_ptr<PMImpl> pNew(new PMImpl(*pImpl)); // copy obj.data
  pNew->bgImage.reset(new Image(imgSrc));     // modify the copy
  ++pNew->imageChanges;
  swap(pImpl, pNew);                          // swap the new
                                              // data into place
}
```

- Does **not matter** how many objects have changed.
- As long as **dont do** the *last swap step*, objects **remain** in *their original state*.
- `copy-and-swap` comes with a **cost** of **copying the object**.

---

- Another *very important* is that *even* the **function** you are *calling* provides **strong exception safe guarantee**
  - Does *not mean* your **function automatically** provides **exception safe guarantee**
    - Example

```c++
void someFunc()
{
  ...   // make copy of local state
  f1();
  f2();
  ...   // swap modified state into place
}
```

- If **both** $f1$ and $f2$ provide **strong exception safe guarantee** and $f1$ executed without any **exception**
  - Alongside some **modified database value**
    - Then $f2$ throws **exception**

---

- How can `someFunc` restore **database modification** to the **original value**
  - This is *hard* and nearly **impossible to achieve**.

- Issue is **side effects**
  - As long as **functions** operate on *local state*
    - example $\to$ $someFunc$ affects only the **state of object** from which its **invoke**
  - Its easy to **offer strong guarantee**
- Functions have **side effects** on *non local data* $\to$ harder to **offer strong guarantee**.

## Item 30 - Understand ins and outs of inlining

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220410123940085.png)

> -- key points --
>
> - Inline replace function calls with **code body.**
>
> - Uses up lots of **memory** thus consider when there is **limited memory**.
>
>   - reduced instruction cache hit rate, virtual memory, **inline reduced code bloat**
>   - performance penalties amongst these
>
> - If inlines are **short** $\to$ code generated may lead to **smaller object code** and *highest instruction cache hit rate*.
>
> - Request and *not commands* the **compilers**.
>
>   - Given *explicitly* or *implicitly*
>     - Implicitly are often **member functions**
>     - Explicit uses `inline` keyword.
>
> - `inline` and `template` are often defined in the **header file**.
>
> - Inline functions must be on templates is an **invalid** and **potentially harmful**, thus its worth looking into.
>
> - Often perform *inlining* during **compilation** (may be done during linking/ runtime)
>
>   - Replace function call with the **body of the called function**.
>     - Compilers must **know** what the *function* looks like for this 
>
> - Template **instantiation** is *independent* of *inlining*
>
>   - Avoid inlining most of the time for short functions.
>
> - Compilers ignore complex inline requests.
>
>   - Recursive / Loops.
>   - Only basic virtual function calls 
>     - Virtual means **wait for runtime to figure out the function to call**.
>
> - `inline` depends on the **build environment** in question, primarily the compiler.
>
>   - Compilers have a **diagnostic level** that results in warning if they *fail to inline* a **function** that we *asked for*.
>
> - If we take the **address** of an *inlined* function then they **ignore the inline**.
>
>   - Cannot generate a **pointer** without the function in place.
>
> - Compilers dont **typically** perform the **inlining** across calls via **function pointers**
>
>   - This means that *calls to an inline function* may/may not be inlined
>
>     - Dependent on **how the calls are made**.
>
>     ```c++
>     inline void f(){} // assume compilers are willing to inline calls to f
>     void (*pf)() = f; // pf points to f
>     ...
>     f(); // call inlined as its normal
>     pf(); // probably wont be as its via a function pointer.
>     ```

```c++
class Person {
    public:
    	int age() const {return theAge;} // implicit inline request, age defined in the class definition
    private:
    	int theAge;
}
```

```c++
template<typename T>
inline const T& std::max(constT& a const T& b){ // explicit inline, request std::max is preceded inline
    return a < b ? b : a;
}
```

- `max` is a template brings observation that **both inline functions** and **templates** are *typically defined* in *header files*.

## Item 31 - Minimise compilation dependencies between files

> - Seperate a class into two classes, one is just an *interface*, one is just an *implementation* of that interface.
>
> - If the **implementation** class is *named* `PersonImpl` , Person defined as such where its private member variable is a **shared pointer** to the `PersonImpl` , using the **pimpl** idiom.
>
> - We never *forward declare* any **standard library classes**, as there are complex intricacies to consider.
>
> ---
>
> - Key idea is replacement of dependencies on **definitions** with *dependencies* on *declarations*, this is the idea of minimising dependencies.
>
> --- key points --
> 
> - Avoid using objects when object references and pointers will do
> 
>   - you can **define references** and **pointers** to the type with only a *declaration for the type*
>   - Defining objects of a **type necessitates** the presence of the **types definition**.
>
> - Depend on *class declarations* instead of *class definitions*
>
>   - We never need a class definition to declare a function using this class, not even if the function passes or returns the class type by value
>
>   - ```c++
>    class Date; // class declaration
>     Date today(); // fine - no definition required
>      void clearAppointments(Date d);
>     ```
>
> - Provide **seperate header files** for *declarations* and *definitions* 
>
>   - ```c++
>    #include "datefwd.h" // header file declaring (but not defining) class Date
>                                 
>     Date today(); // as before.
>      void clearAppointments(Date d);
>     ```

- Key idea is the **pointer to implementation file** from which includes all the **dependencies** from external libraries for member variables
- The implementation can be edited without the clients using the interface being recompiled each time.

```c++
#include <iostream>
#include "User.h"

int main(){
    std::string name {"jake"};
    User h {std::move(name)};
    std::cout << h.getName() << std::endl;
}
```

```C++
//UserImpl.h

#include <string>
class UserImpl {
public:
    explicit UserImpl(std::string&& n): name{n}{}

    [[nodiscard]] std::string getName() const {return name;}
private:
    std::string name;
};
```

```c++
// User.h

#include <memory>
#include <string>
class UserImpl;

class User {
public:
    explicit User(std::string&& n);

    [[nodiscard]] std::string getName() const;

    User();
private:
    std::unique_ptr<UserImpl> pImpl;
};
```

```c++

// User.cpp

#include "User.h"
#include "UserImpl.h"

/* testing out R value references, keep r value by moving*/
User::User(std::string&& n): pImpl {std::make_unique<UserImpl>(std::move(n))}{}

std::string User::getName() const {
    return pImpl->getName();
}

/* declare default destructor unique_ptr to see on the complete type declaration*/
User::~User = default;
```

- Alternative to using a *handle class* and `impl class` style is to make an `interface class`. 
  - Contains virtual destructors
  - no data members
  - no constructors
  - pure virtual functions for the interface.

## Item 32 - Ensure public inheritance models is-a

- Basically only *derive* when the *derived class* IS-A *base class* but *not the other way around*.
  - Holds for *Public inheritance only*.
- *Private inheritance* is different as mentioned in item 39, also *protected inheritance* means something different.
- Ensure the *inheritance* hierarchy holds for *all cases* of the *derived*
  - Example is a `fly` method in `bird` class when not all birds can actually fly such as a `penguin`.
  - Could generate a **runtime error** instead.
- public inheritance asserts that everything applied to base class , applies to all derived classes .

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220605134925361.png)

## Item 33 - Avoid hiding inherited names

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220605135512353.png)

- Derived class scope of each of the functions.
- Compile searches from inside function for mf2 call, then to derived scope, then to base , finds here.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220605140359296.png)

- This shadows the overloaded mf1 and mf3 double / int functions as they are seen to not exist at all so this causes an error:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220605140434003.png)

- This technically violates the *is-a* model.
- We override this by bringing the scope into the one desired by `using`. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220605140937902.png)

- Private inheritance may not want to get everything
- Only want a specific type of mf1 taking no parameters.
- Make a forwarding function

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220605141722181.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220605141740079.png)

## Item 34 - Differentiate between inheritance of interface and inheritance of implementation

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725170642647.png)

> - Public inheritance has interface / implementation considerations.
> - choose whether implementation are overriden or not.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220621002307149.png)

- Shape is a pure virtual function.
- Must inherit shape to instantiate.
- pure virtual function only purpose is to have derived classes inherit a function interface only.
- The idea of drawing can not be generalised in any way so its a pure virtual function.

---

- Can define an abstract virtual function definition but must be called by qualifying the name.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725142947012.png)

---

- Purpose of **simple virtual functions** is to inherit an interface along with the **default implementation**.

- Dangerous idea to have **simple virtual functions** that specify both a **function interface** and **default implementation**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725153108825.png)

- Create an error function but there is a default to fall back on if you are required.
- Move the common feature into the base class which makes logical sense.
- Does not make sense a new plane comes in which flies in a different way
- Issue is that the new model of plane being used was **allowed to inherit** this behaviour without explicitly saying it wanted it.
- We therefore **sever the connection** between the **interface** of the **virtual function** and the **default implementation**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725162033101.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725162217229.png)

- There is no way to accidentally inherit the wrong method as it creates its own fly without associating with the default fly method.
- ensure the default fly is non virtual obviously as nothing should override its functionality.
- This can be changed to actually just have an implementation directly for a pure virtual function 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725162622979.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725162627534.png)

- Thus they have the option to fallback on default or use their own implementation.
- Lose the ability to select protection methods in terms of private / public.

---

- Non virtual member functions specifies an **invariant over a specialization** $\to$ identifies behaviour that is **not supposed to change.**
- Purpose of declaring **non virtual function** is to have derived classes inherit a function interface as well as some **mandatory implementation**.

---

- The difference of simple , pure, virtual and non virtual functions enables **precision** in what the **derived classes** must **inherit**
  1. Interface only
  2. Interface and default implementation
  3. Interface and mandatory implementation.
- All base classes should have some **virtual member functions** present in them.

---

- 80/20 rule - 80% of time is spent **executing** just 20% of the code.
- Focus on the 20% where performance is important as 80% of virtual calls are okay.

---

- Ensure some kind of invariant as in a non virtual member rather than have everything being virtual which lacks backbone in the application.

## Item 35 - Consider alternatives to virtual functions

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725183607549.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725183616277.png)

- Health bar made virtual as characters calculate health in different ways.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725171253405.png)

- There is **no pure virtual function** therefore it suggests a default implementation.

- Alternative to this

### Template method pattern via non virtual interface idiom.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725171506439.png)

- public function calling private virtual function.
- This is the **non virtual interface idiom**.
- Manifestation of the general design pattern called the **template method** (nothing to do with c++ templates).
- The **non virtual function** acts as the **wrapper** for the **virtual function**.

---

- Advantage of this is do **before / after** stuff which are guaranteed to be set before and after virtual calls such as **proper context setup** and **cleanup**.
- Before may be
  1. Mutex locking
  2. logging
  3. verifying class invariants
  4. function preconditions are satisified.
- After may be
  1. Mutex unlocking.
  2. verifying function postconditions
  3. reverification of class invariants.
- This is not effective is the **client** calls the **virtual functions directly**.

---

- The users implementing the class actually specify a function to change which they never actually directly call which is fine.
- private virtual functions can be implemented in this sense because its an interface that expects this.

---

- Idiom may use protected methods to enable derived calling base.
- Public may be used for example the destructors in polymorphic base class but the NVI method isn't really applied in this sense.

### The strategy pattern via function pointers.

- Design choice is that calculation of a characters health is independent of the characters type
  - Calculations are **not part of the character at all**.
- Constructor passed a **function pointer** to a health calculator and you **call that function**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725173327195.png)

- Application of the **strategy design pattern**.
- This offers some **flexibility**
  1. Different instances of the **same character type** may have **different health calculations**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725173734328.png)

2. Health calculation functions for a **particular character** may be **changed at runtime**

- There is **no special access** to the class internals as its just a separate function, fine if **only public variables** are required to calculate.
- Issue is when you change class member functions without changing the other functions.

---

- Only way to prevent need for non member functions is to **weaken encapsulation**.
- Declare the **non member functions** as being **friends**.
- Offer **public accessor** functions for **part** of **its implementation**  
- The balance between **losing encapsulation** and **versatility** of **function pointers** over the **virtual functions** s a **design by design basis**.

#### Strategy pattern using std::function

- Use something that acts **like a function** rather than **directly a function**
- Any callable object such as **functor** and have **convertible to int** properties rather than **strict int**.

---

- These constraints are gone when using **std::function** a

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725175608132.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725175613049.png)

- A generalized function pointer that accepts interfacing matching the `int(const GameCharacter&)` signature.
- More flexibility:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725180016438.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725180023966.png)

- bind is used as the health function passes in a `GameLevel` object alongside any other parameters.
- Game character only takes a pointer to the function calling it as parameter
- Game Level takes a game character and a pointer to the game level.
- The bind will ensure the game level created is passed in as the function of which takes the shape of the health calculator std::function, in sense meaning it takes a single function as a whole.

---

### Classic strategy pattern

- Alternative health calculation is make the health calculator a **hierarchy** of its **own**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725182749094.png)

- Game Character objects always contain a pointer to the healthcalcfunc.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220725183028519.png)

## Item 36 - Never redefine inherited non virtual function

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220727214259425.png)

- Derived class that inherits a member function can create a function with the same name and this shadows the name above it.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220727204813741.png)

- Virtual functions are **dynamically bound** $\to$ they do **not suffer** from this problem
- If $mf$ were a **virtual function**  $\to$ call to $mf$ through pB / pD invokes `D::mf` as they both point to an **object of type D**.

```c++
D x;

B *pB = &x;
D *pD = &x;
```

- Derived objects redefining a non virtual inherited function shows us **inconsistent behaviour** acting like base/derived in unexpected manners.
- References behave in a **odd manner** as to **pointers**.

---

- Everything applied to the $B$ class applies to the $D$ objects, as $D$ is-a $B$. 
- Classes derived from $B$ inherit the **interface** and the **implementation** of $mf$ as $mf$ is **non virtual** within $B$. 

---

- Every $D$ is **not a B** if D needs to change $mf$ and each $B$ object has the use the **B implementation** of $mf$.
- D **should not inherit from B** in a *public manner*.
- mf does **not reflect an invariant** if $D$ actually requires the $B$ class and implement mf differently, thus $mf$ should be **virtual**.
- If each $D$  **is a B** and $mf$ corresponds to an invariant over **specialization** for $B$, then $D$ can **not honestly** need to *redefine mf* and should **not attempt**.

---

- This relates to the virtual destructors being need in polymorphic classes so the non virtual inherited destructor does not redefine 
- Destructors are compiler generated so each derivation would redefine it.

## Item 37 - Never redefine a functions inherited default parameter value

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220727223828213.png)

- Only refer to virtual functions as you never redefine a non virtual function.
- Virtual functions are **dynamically bound** but **default values** are *statically bound*.

---

- A *static type* is the **type** declared in the **program text**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220727214953654.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220727215000399.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220727215033851.png)

- The **dynamic type** is the value that the **pointer** actually holds whereas the **static type** is $Shape$ always.
- Dynamic type determines the **behaviour**. 
- This static binding of **default parameters** means using a **derived function** but with **default parameters** from the **base class** potentially.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220727221616768.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220727221621118.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220727221625334.png)

- References would **not change this issue**.

---

- This comes down to **runtime efficiency**. If **default parameters** were **dynamically bound**, compilers have work to **come up with a method** to determine appropriate default values for **parameters** of **virtual functions** at the **runtime** $\to$ much **slower** and **more complicated**.

---

- Offering default parameter values for both base/derived classes.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220727222957302.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220727223003622.png)

- This is **a code duplication** that comes with **dependencies** $\to$ change shape value , change all.

---

- Consider alternatives design such as the **non virtual interface idiom**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220727223425750.png)

- non virtual functions **should not be redefined by derived by derived classes** $\to$ this design makes it clear that default value for draws **color parameter** should *always be red*.

## Item 38 - Model "has a" or "is-implemented-in-terms-of" through composition.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220728194625696.png)

- **Composition** $\to$ relationship between types that **arises** when *objects* of *another type*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220728190040433.png)

- This means it possesses a **has-a** or **is-implemented-in-terms-of** as there are **two domains in the software**.
- Some objects are things like *people/vehicles* and others are **implementation artifacts** such as *mutexes / search trees*.
- implementation domain $\to$ is implemented in terms of.
- application domain $\to$ has a.

---

- Trade-offs in the implementation domain must be considered such as `std::set` for a collection preventing duplicates which has speed preference over space which is **not what we want.**

- use `std::list`  instead to implement your own `std::set`.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220728191243138.png)

- This does not make sense as a set cant be a list as a list contains duplicates.
- Therefore **public inheritance** is the **wrong method** to use for this. Rather perform an **implemented in terms of** method:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220728194121373.png)

- Set depends a lot on this list with its already implemented functionality.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220728194334944.png)

- these are reasonable candidates for **inline**. 

## Item 39 - Use private inheritance judiciously.

- Public inheritance in c++ is *an is-a* therefore can implicit convert derived to base.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220728195115735.png)

- in private inheritance, compilers do **not perform implicit conversions**.
