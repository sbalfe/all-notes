# Function Templates

## First Look Function Templates

- You can use **any type** (fundamental type, class, and so on) as long as it **provides the operations** that the **template uses**.

```c++
template <typename T> 
T max (T a, T b) {
	return b < a ? a : b;
}
```

- Type $T$ must support $<$ and be **copyable** (C++17 , can pass *rvalues*) 
- Always prefer `typename` over `class`. 

- Replacing template **parameters** by *concrete types* is **==instantiation==** $\to$ *instance* of some *template*.
- `void` is a *valid* template argument

```c++
template<typename T>
T foo(T*){}

void* vp = nullptr;
foo(vp); // deduces void foo(void*);
```

### Two Phase translation

- **Compile time error** attempting to **instantiate** templates with *invalid types* that dont **support operations**.

```c++
std::complex<float> c1, c2; // no < 
::max(c1,c2); // compile time error
```

- Two phases
  1. No instantiation at *definition time*
     - Code checked for correctness
     - Syntax errors found
     - Using unknown names
     - Static assertions independent of parameters checked
  2. At *instantiation time*
     - Check template code again 
     - Parts *dependent* on *template parameters* are double checked.

```c++
template<typename T>
void foo(T t)
{
    undeclared(); // first-phase compile-time error if undeclared() unknown
    
    undeclared(t); // second-phase compile-time error if undeclared(T) unknown
    
    static_assert(sizeof(int) > 10, // always fails if sizeof(int)<=10
    "int too small");
    
    static_assert(sizeof(T) > 10, // fails if instantiated for T with size <=10
    "T too small");
}
```

- This is a **two phase lookup**. 
- Compilers may **not perform full checks** on the *first phase*.
- Issues with compiling and linking regarding this refer to *chapter 9 - templates book*.

## Template Argument deduction

- Some type $T$ may form *part* of a type

```c++
template<typename T>
T max(const T& a , const T& b){
    return b < a ? a : b;
}
```

- Passing `int` deduces `T` as an `int` as it matches for `const int&` 

- Call parameters by **reference** $\to$ *trivial conversions* do not apply to **type deduction**
- Two arguments declared with **same template parameter** $T$ must *exactly match*
- Call parameters by **value** $\to$ trivial conversions *decay* $\to$ $cv$ qualifiers ignored, references convert to referenced type, raw arrays/functions convert to the **pointer type** 
- The *decayed types* must match for same template parameter $T$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220323044736121.png)

- Three methods to fix

  1. Cast arguments 

  ```c++
  max(static_cast<double>(4), 7.2)
  ```

  2. Specify or qualify explicitly the type of $T$ preventing compiler from **attempting type deduction**.

  ```c++
  max<double>(4,7.2);
  ```

  3. Specify parameters may have different types.

### Type deduction for default arguments

- Type deduction does **not work** for *default* call arguments

```c++
template<typename T>
void f(T = "");

f(1); // deduced T to be int, to call f<int>(1)
f(); // cannot deduce T
```

- Declare default argument for **template parameter too**

```c++
template<typename T = std::string>
void f(T = "");

f(); // OK
```

## Multiple template parameters

- Have **various template parameters**

```c++
template<typename T1, typename T2>
T1 max (T1 a, T2 b)
{
return b < a ? a : b;
}
...
auto m = ::max(4, 7.2); // OK, but type of first argument defines return type
auto m2 = ::max(66.66, 7); // 66.66
auto m3 = ::max(7,66.66); // 66
auto m4 = ::max<double>(66.66, 7) // force type conversion
```

- Introduce **third template** parameter to *declare return type.*
- Make the **compiler** find out the **return type**.
- Declare the **return type** to be *common* of the **two parameter types**.

### Template Parameters for Return types

```c++
template<typename T1, typename T2, typename RT>
RT max (T1 a, T2 b);
```

- Template **argument deduction** does not take return types into account as it does *not appear* in **function call parameters**, thus no deduction.
- Must specify **template argument list explicitly**

```c++
::max<int, double, double>(4 , 7.2)
```

- Specify only **first arguments** 

```c++
template<typename RT, typename T1, typename T2>
RT max (T1 a, T2 b);
...
    
// T1 = int, T2 = double.
::max<double>(4, 7.2) // OK: return type is double, T1 and T2 are deduced
```

### Deducing return type

```c++
template<typename T1, typename T2>
auto max (T1 a, T2 b){
    
	return b < a ? a : b;
}
```

- Using **no trailing return type** `->` means the *expression* in return **deduces** the *type*.
- Deduction from **function body** must be possible, code is *available* and **multiple return statements** *must match*.

- Use **==trailing return type==** enabling to use **call parameters**
- We can **declare** the return type is *derived* from what `operator?` yields

```c++
template<typename T1, typename T2>
auto max (T1 a, T2 b) -> decltype(b<a?a:b)
{
	return b < a ? a : b;
}
```

- Must locate a **common return type** from the *rules* defined by `?:`. 

----

> ```c++
> /* This is a declaration, compiler uses the decltype rules*/
> template<typename T1, typename T2>
> auto max (T1 a, T2 b) -> decltype(b<a?a:b);
> ```
> - This may have **a return type** that is *reference type*, as under some conditions $T$ can be a *reference.* 
> - We thus use type `decayed` from `T` 
>
> ```c++
> #include <type_traits>
> template<typename T1, typename T2>
> auto max (T1 a, T2 b) -> typename std::decay<decltype(true?a:b)>::type
> {
> 	return b < a ? a : b;
> }
> ```
>
> - `std::decay<>` $\to$  returns **resulting type** in a *member type* (removes *reference*)
> - Must have `typename` keyword to **qualify** this *expression* and **access**

---

- Initialisation of `auto` type **always decays** $\to$ Also applies to **return types**. 
- `auto` as a return type behaves just as in the following code, where $a$ is **declared** by the **decayed type** of $i$ , $int$ 

```c++
int i = 42;
int const & ir = i; // ir refers to i
auto a = ir; //  a declared as new object of type int

```

### Return Type as Common Type

- `std::common_type` obtains the common type shared between the two types.


```c++
typename std::common_type<T1, T2>::type; // Since C++11
 std::common_type_t<T1,T2> // since C++14
```

```c++
#include <type_traits>
template<typename T1, typename T2>
std::common_type_t<T1,T2> max (T1 a, T2 b)
{
	return b < a ? a : b;
}
```

- Selects type based on the *language rules* of *operator* `?:` or *specializations* for specific types.
- It *decays* also.

## Default Template Arguments

- The **==default template arguments==** used in any template.
- Combine ability of defining return type to have multiple parameter types

```c++
#include <type_traits>

/* set this defaulted to the decayed version of the expression */

/* implementation requires the default constructor ability to decltype */
template<typename T1, typename T2, typename RT = std::decay_t<decltype(true ? T1() : T2())>>
RT max (T1 a, T2 b)
{
	return b < a ? a : b;
}
```

```c++
#include <type_traits>
template<typename T1, typename T2,
/* obtain the type using std::commontype_t easily, decays automatically. */
typename RT = std::common_type_t<T1,T2>>
RT max (T1 a, T2 b)
{
return b < a ? a : b;
}
```

- This enables us to easily have deduced return type

```c++
auto a = ::max(4,72);
```

- Specify return type after all other arguments type explicitly

```c++
auto b = ::max<double,int,long double>(7.2, 4);
```

```c++
/*
	Make the first parameter the return type
*/
template<typename RT = long, typename T1, typename T2>
RT max (T1 a, T2 b)
{
	return b < a ? a : b;
}

int main(){
    max(i,l); // return long (default arg of template parameter for return type)
    max<int>(4,42) // return int
}
```

- Only makes sense the above approach from a natural default parameter for the return types.

## Overloading function templates

- Function templates *can be overloaded*.
- Define **the same function name** with *different definitions*

```c++
// maximum of two int values:
int max (int a, int b)
{
	return b < a ? a : b;
}

// maximum of two values of any type:
template<typename T>
T max (T a, T b)
{
	return b < a ? a : b;
}
int main()
{
    ::max(7, 42); // calls the nontemplate for two ints
    ::max(7.0, 42.0); // calls max<double> (by argument deduction)
    ::max('a', 'b'); // calls max<char> (by argument deduction)
    ::max<>(7, 42); // calls max<int> (by argument deduction)
    ::max<double>(7, 42); // calls max<double> (no argument deduction)
    ::max('a', 42.7); // calls the nontemplate for two ints
}
```

- `::max<>(7,42)` is an *explicit empty template parameter list* to indicate that only templates may resolve this call
- The template parameters are still deduced from the call arguments.
- `::max(7.0,42.0)` is a *better match* than the `int` non template function as there is **no conversion required**, same as for the `char` example above.
- `::max('a', 42.7)` only the non templated version allows **non trivial conversions**.

---

```c++
template<typename T1, typename T2>
auto max (T1 a, T2 b)
{
	return b < a ? a : b;
}

/* overloading the max to explicitly specify the return type only */
template<typename RT, typename T1, typename T2>
RT max (T1 a, T2 b)
{
	return b < a ? a : b;
}
```

```c++
auto a = ::max(4, 7.2); // uses first template
auto b = ::max<long double>(7.2, 4); // uses second template
auto c = ::max<int>(4, 7.2); // ERROR: both function templates match
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220413010907482.png)

- Important to ensure only one call matches to a specific template.

---

```c++
#include <cstring>
#include <string>

// maximum of two values of any type:
template<typename T>
T max (T a, T b)
{
	return b < a ? a : b;
}
// maximum of two pointers:
template<typename T>
T* max (T* a, T* b)
{
	return *b < *a ? a : b;
}
// maximum of two C-strings:
char const* max (char const* a, char const* b)
{
	return std::strcmp(b,a) < 0 ? a : b;
}

int main ()
{
    int a = 7;
    int b = 42;
    auto m1 = ::max(a,b); // max() for two values of type int
    
    std::string s1 = "hey";
    std::string s2 = "you";
    auto m2 = ::max(s1,s2); // max() for two values of type std::string
    
    int* p1 = &b;
    int* p2 = &a;
    auto m3 = ::max(p1,p2); // max() for two pointers
    
    char const* x = "hello";
    char const* y = "world";
    auto m4 = ::max(x,y); // max() for two C-strings
}
```

- Overload of max above *passes by value*
- Limit changes of overloading function templates or explicit template parameter use.
- If we implement `max()` to pass arguments by **reference** and *overload* on *two C strings* passed by value, we **cannot use** the *three argument version* to compute the maximum of *three C strings*.

```c++
#include <cstring>

// maximum of two values of any type (call-by-reference)
template<typename T>
T const& max (T const& a, T const& b)
{
	return b < a ? a : b;
}

// maximum of two C-strings (call-by-value)
char const* max (char const* a, char const* b)
{
	return std::strcmp(b,a) < 0 ? a : b;
}

// maximum of three values of any type (call-by-reference)
template<typename T>
T const& max (T const& a, T const& b, T const& c)
{
	return max (max(a,b), c); // error if max(a,b) uses call-by-value
}

int main ()
{
    auto m1 = ::max(7, 42, 68); // OK as temporaries are created in the main function which is safe.
    char const* s1 = "frederic";
    char const* s2 = "anica";
    char const* s3 = "lucas";
    auto m2 = ::max(s1, s2, s3); // run-time ERROR
}
```

- When we call `max()` for three C strings

```c++
return max(max(a,b), c)
```

- Is a **runtime error** 
- `max(a,b)` creates a **temporary local value** returned by *reference*, this of course *expires.* Leaving `main()` with a **dangling reference.**  

> Note this case is actually harmless as we pass the temp object to a another function which returns a reference. The temp object from max(a,b) is referred to in that function and thus creates an lvalue which is completely safe.

- Ensure all overloaded functions of a function are *declared* before the function is called.
  - Not always all visible when a call made.
  - Thus order of declaration actually matters of course.

```c++
#include <iostream>

// maximum of two values of any type:
template<typename T>
T max (T a, T b)
{
    std::cout << "max<T>() \n";
    return b < a ? a : b;
}

// maximum of three values of any type:
template<typename T>
T max (T a, T b, T c)
{
	return max (max(a,b), c); // uses the template version even for ints because the following declaration comestoo late:
}

// maximum of two int values:
int max (int a, int b)
{
    std::cout << "max(int,int) \n";
    return b < a ? a : b;
}
int main()
{
	::max(47,11,33); // OOPS: uses max<T>() instead of max(int,int)
}
```

## Should we overload function templates

### Pass by value or by reference?

- Passing by value is preferred
  - Simple Syntax
  - Compilers optimise better
  - Move semantics make copies cheap
  - Sometimes there is no copy / move at all

- Regarding templates
  - Template may be for simple/complex types thus choosing the complex approach may be *counter productive* for simple types
  - As a caller , we often decide to pass arguments by reference via `std::ref()` and `std::cref()` 
  - Passing string literals or *raw arrays* is a large issue but reference often considered the **larger problem**.

### Inline

https://stackoverflow.com/questions/10535667/does-it-make-any-sense-to-use-inline-keyword-with-templates

https://stackoverflow.com/questions/17667098/inline-template-function - full specializations. 

- Templates do **not have to be called inline**
  - Define in header and include in **many translation units**
- Exception is **full specializations** of templates for **specific types**
- `inline` means a function *can appear multiple times*. Also a *hint* to compiler to expand the function inline.
  - Producing **more efficient code** in *some cases*.
- Compilers often are better at using *no hint* even.

### Why not constexpr?

- Makes sense in some cases to evaluate at *compile time*.

```c++
template<typename T1, typename T2>
constexpr auto max (T1 a, T2 b)
{
	return b < a ? a : b;
}
```

- We can use this for example in declaring the **sizes** of *arrays* at *compile time*

```c++
int a[::max(sizeof(char),1000u)];
```

- Alternatively for the **size** of a `std::array<>` 

```c++
std::array<std::string, ::max(sizeof(char),1000u)> arr;
```

- We pass 1000 as **unsigned int** to avoid warnings about comparing *signed* with *unsigned* in template.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220817154910431.png)

# Class Templates

## Implementation of class template `stack` 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220817161129381.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220817161220274.png)

- Uses `std::vector` therefore we don't care about **memory management , copy constructors and assignment operators**.

### Declaration of class templates

```c++
template<typename T>
class Stack {
    ...
};
```

- Inside the class function template $\to$ `T` used just like other types to **declare members** and **member functions**.
- Above T is used for **elements** of the vector.
- Must use `stack<T>` unless some element of **deduction can occur**.

```c++
template <typename T>
class Stack {
    ...
    Stack (Stack const&); // copy constructor
    Stack& operator=(Stack const&); // assignment operator
}
```

- This implies Stack being used with the **parameters** that instantiated it.
- using `<T>` signifies a handle of **special template parameters** therefore use the **first form**.

---

- Outside the **class structure** $\to$ requires:

```c++
template <typename T>
bool operator==(Stack<T> const& lhs, Stack<T> const& rhs);
```

---

- Cannot declare **template classes** or define **class templates** inside **functions / block scope**.
- Templates can only be defined in **global / namespace** scope or **inside class declarations**.

### Implementation of member functions

- Use template keyword and use the full type qualification for  a class template

```c++
template<typename T>
void Stack<T>::push (T const& elem)
{
	elems.push_back(elem); // append copy of passed elem
}
```

- Impossible to implement exception safe `pop()` that returns the **removed element**

- Copy and return copy instead.

```c++
template<typename T>
T Stack<T>::pop ()
{
    assert(!elems.empty());
    T elem = elems.back(); // save copy of last element
    elems.pop_back(); // remove last element
    return elem; // return copy of saved element
}
```

- usage error of using pop when empty we must assert.

---

- Implement as **inline functions also**

```c++
template <typename T>
class Stack {
    ...
    void push (T const& elem){
        elems.push_back(elem); // append copy of passed elem
    }
}
```

## Using a class template `stack` 

```c++
#include "stack1.hpp"
#include <iostream>
#include <string>

int main()
{
    Stack<int> intStack; // stack of ints
    Stack<std::string> stringStack; // stack of strings
    // manipulate int stack
    intStack.push(7);
    std::cout << intStack.top() << '\n';
    // manipulate string stack
    stringStack.push("hello");
    std::cout << stringStack.top() << '\n';
    stringStack.pop();
}
```

- C++17 can derive from the constructor parameters the type used.

---

- Only the **member functions** that are called are **instantiated**.
- Use class templates **only partially**.

---

- Class having a **static members** $\to$ instantiated once for **each type** for which **class template is used**.
- Use it just like any other type with `const` / `volatile` , derive **array** and **reference** types from it. 
- use `typedef` or `using` alias too

```c++
void foo(Stack<int> const& s) // parameter s is int stack
{
	using IntStack = Stack<int>; // IntStack is another name for Stack<int>
	Stack<int> istack[10]; // istack is array of 10 int stacks
	IntStack istack2[10]; // istack2 is also an array of 10 int stacks (same type)
	...
}
```

## Partial usage of class template

- Template arguments only provide necessary operations that are **needed** and **not could be needed**.

```c++
template<typename T>
class Stack {
		...
        void printOn() (std::ostream& strm) const {
        for (T const& elem : elems) {
        strm << elem << ' '; // call << for each element
		}
	}
};
```

- Use this class for **elements** that have **no** `operator<<`.

```c++
Stack<std::pair<int,int>> ps; // note: std::pair<> has no operator<< defined
ps.push({4, 5}); // OK
ps.push({6, 7}); // OK
std::cout << ps.top().first << '\n'; // OK
std::cout << ps.top().second << '\n'; // OK
```

- Only calling `printOn` for **such a stack** $\to$ code produces an **error**, because it cannot **instantiate** the call of *operator<<* for this **specific element type.**

### Concepts

- How do we know which operations are **required** for a template to be **instantiated**.
- The idea of a *concept* $\to$ denotes a set of **constraints** that is *repeatedly* required in the **template library**.
- C++ STL relies on concepts such as *random access iterator* and *default constructible*.

----

- As of c++11 can use basic **static asserts**

```c++
template<typename T>
class C
{
    static_assert(std::is_default_constructible<T>::value,
    "Class C requires default-constructible elements");
    ...
};	
```

## Friends

- `operator <<` is implemented as **non member function** which calls the `printOn` inline.

```c++
template<typename T>
class Stack {
	...
	void printOn() (std::ostream& strm) const {
		...
	}
	friend std::ostream& operator<< (std::ostream& strm,
       								 Stack<T> const& s) {
        s.printOn(strm);
        return strm;
	}
};
```

- `operator<<` is not  a function template, just an ordinary one with the **class template as required**.

---

- Trying to *declare / define* friend function requires **two options**

  1. Implicitly declare **new function template**, uses a different template parameter such as `U`.

   ```C++
     template<typename T>
     class Stack {
         ...
         template<typename U>
         friend std::ostream& operator<< (std::ostream&, Stack<U> const&);
     };
   ```
  - Neither using `T` again **nor skipping** the **template parameter** declaration would work
  - Inner `T` hides the `outer` T or we declare a **nontemplate function** within the **namespace scope**.

2.  **Forward declare** the output operator for `Stack<T>` to be a template, first forward declare `Stack<T>`

```c++
template <typename T>
class Stack;
template <typename T>
std::ostream& operator<<(std::ostream&, Stack<T> const&);
```

- Then we **declare this function** as *friend*.

```c++
template <typename T>
class Stack {
    ...
    friend std::ostream& operator<< <T> (std::ostream&, Stack<T> const&);
}
```

- declare a **specialization** of the *nonmember function* template as a **friend**.
- Without `<T>` we would declare a **new nontemplate function**

- Still use this class with elements that **dont have ** the *operator<<* defined.
- Only calling `operator<<` reports an **error**

```c++
Stack<std::pair<int,int>> ps; // std::pair<> has no operator<< defined
ps.push({4, 5}); // OK
ps.push({6, 7}); // OK
std::cout << ps.top().first << '\n'; // OK
std::cout << ps.top().second << '\n'; // OK
std::cout << ps << '\n'; // ERROR: operator<< not supported
// for element type
```

## Specialisations of class templates

- Specialize a **class template** for *certain template arguments*. 
- Provide special implementations for **particular circumstances**.
- Some template parameters **must be defined** by the **user**.

```c++
#include "stack1.hpp"

//partial specialization of class Stack<> for pointers:

template <typename T>
class Stack<T*>{
    private:
    	std::vector<T*> elems; // elements
    public:
    	void push (T*); //push element
    	T* pop(); // pop element
    	T* top() const; // return top element
    	bool empty const { // return whether the stack is empty
            return elems.emptyh(); 
        }
};

template<typename T>
void Stack<T*>::push (T* elem)
{
	elems.push_back(elem); // append copy of passed elem
}
template<typename T>
T* Stack<T*>::pop ()
{
    assert(!elems.empty());
    T* p = elems.back();
    elems.pop_back(); // remove last element
    return p; // and return it (unlike in the general case)
}
template<typename T>
T* Stack<T*>::top () const
{
    assert(!elems.empty());
    return elems.back(); // return copy of last element
}
```

```c++
template <typename T>
class Stack<T*>{
    
};
```

- Define a **class template** that is *parameterized* for `T` but *specialized* for a **pointer** `Stack<T*>` 
- May provide a **different interface**.

---

- In this example $\to$ `pop()` returns the **stored pointer** $\to$ so that user of **class template** can call `delete` for the **removed value** when it was *created with* `new` 

```c++
Stack<int* > ptrStack; //stack of pointers (special implementation)

ptrStack.push(new int {42});
std::cout << *ptrStack.top() << '\n';
delete ptrStack.pop();
```

### Partial specialization with multiple parameters

- Class templates **may specialize** the *relationship* between **multiple template parameters**
- Example:

```c++
template <typename T1, typename T2>
class MyClass {
    ...
};
```

- Following **partial specializations** are *possible*:

```c++
// partial specialization: both template parameters have same type
template <typename T>
class MyClass<T,T>{
    ...
};

// partial specialization: second type is int
template<typename T>
class MyClass<T,int> {
	...
};

// partial specialization: both template parameters are pointer types
template<typename T1, typename T2>
class MyClass<T1*,T2*> {
	...
};
```

- The following examples show which templates is used by **which declaration**.

```c++
MyClass<int , float> mif; // uses MyClass<T1,T2>
MyClass<float, float> mff; // uses MyClass<T,T>
MyClass<float, int> mfi; // uses MyClass<T,int>
MyClass<int*,  float*> mp; // uses MyClass<T1*, T2*>
```

- If more than **one partial specialization** matches equally well, the **declaration is ambiguous**.

```c++
MyClass<int,int> m; // ERROR: matches MyClass<T,T>
// and MyClass<T,int>
MyClass<int*,int*> m; // ERROR: matches MyClass<T,T>
// and MyClass<T1*,T2*>
```

- To resolve the **second ambiguity** $\to$ provide additional partial specialization for **pointers**.

```c++
template <typename T>
class MyClass<T*, T*>{
  ...  
};
```

## Default Class template arguments

- Define default **values** for class template as with **function templates**.

```c++
#include <vector>
#include <cassert>
template<typename T, typename Cont = std::vector<T>>
class Stack {
    private:
        Cont elems; // elements
    public:
        void push(T const& elem); // push element
        void pop(); // pop element
        T const& top() const; // return top element
        bool empty() const { // return whether the stack is empty
        	return elems.empty();
        }
};

template<typename T, typename Cont>
void Stack<T,Cont>::push (T const& elem)
{
    elems.push_back(elem); // append copy of passed elem
}

template<typename T, typename Cont>
     void Stack<T,Cont>::pop ()
{
    assert(!elems.empty());
    elems.pop_back(); // remove last element
}

template<typename T, typename Cont>
T const& Stack<T,Cont>::top () const
{
    assert(!elems.empty());
    return elems.back(); // return copy of last element
}
```

- Member functions must now be defined with **these two parameters**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220818191226105.png)

- Use this **stack** the same way it **was used before**
- Thus passing in a single element , the **vector** is used as default.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220818191333291.png)

- Specify container for the **elements** when you declare `Stack` object in the **program.**

```c++
#include "stack3.hpp"
#include <iostream>
#include <deque>
int main()
{
    // stack of ints:
    Stack<int> intStack;
    
    // stack of doubles using a std::deque<> to manage the elements
    Stack<double,std::deque<double>> dblStack;
    
    // manipulate int stack
    intStack.push(7);
    std::cout << intStack.top() << '\n';
    intStack.pop();
    
    // manipulate double stack
    dblStack.push(42.42);
    std::cout << dblStack.top() << '\n';
    dblStack.pop();
    
}
```

## Type Aliases

- define a **new name** for the **whole type**

### Typedefs and alias declarations

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220818192205578.png)

- Common term is **type alias declaration**.
- Prefer the **alias declaration** , *using*.

### Alias templates

- Unlike *typedef*. Use alias declaration to provide a **convenient name** for a *family of type*
- C++ 11 $\to$ alias template. 

```c++
template<typename T>
using DequeStack = Stack<T, std::deque<T>>; // stack storing element in deque
```

### Alias templates for Member Types

- Alias templates are **helpful** to define **shortcuts** for types that are **members of class templates**

```c++
struct C {
    typedef ... iterator;
    ...
}

// or

struct MyType {
    using iterator = ...;
    ...
};
```

- Definition such as

```c++
template<typename T>
using MyTypeIterator = typename MyType<T>::iterator;
```

- enables 

```c++
MyTypeIterator<int> pos;

// rather

typename MyType<T>::iterator pos; // type
```

### Type Traits Suffix_t

- C++14 $\to$ Standard Library uses this to **define shortcuts** for **type traits** in the **standard library** that *yield a type*.

```c++
std::add_const_t<T> // since C++14
```

- Instead of

```c++
typename std::add_const<T>::type // Since C++11
```

- Standard Library defines:

```c++
namespace std {
    template<typename T> using add_const_t = typename add_const<T>::type;
}
```

## Class Template Argument Deduction

- pre C++17 $\to$ always pass template **parameter types** to *class templates* (unless there are **are default values**).
- Since C++17 $\to$ constraint of specifying the **template arguments** explicitly was **relaxed**.
- Skip to **define templates arguments explicitly** if the *constructor* is able to **deduce** all the *template parameters* (with no default value).

---

- Example $\to$ previous **code examples** $\to$ use copy constructor without specific template arguments. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220818225413103.png)

- Providing constructors that pass **some initial arguments**, you can support **deduction of the element** type of a stack

```c++
template <typename T>
class Stack {
    private:
    	std::vector<T> elems; // elements
    public:
    	Stack() = default;
    	Stack(T const& elem) // initialize stack with one element
            : elems ({elem}){
                
            }
    	...
};
```

- Enables us to declare a **stack as follows**:

```c++
Stack intStack = 0; // Stack<int> deduced since C++17
```

- By initializing the stack with the **integer** $0$ , the template parameter $T$ is deduced to be `int` so a `Stack<int>` is instantiated.

---

- Must have `default` as default constructor implementation as its only available if **no other constructors are defined**
- Use `{elem}` to **initialize** the vector
- Cannot **partially deduce** any *class template arguments*.

### Class Template argument deduction with sting literals

```c++
Stack stringStack = "bottom"; // Stack<char const[7]> deduced since c++17
```

- Issue is this **does not decay** therefore deduction of `char const [7]`. This is as its passed by **reference**
- Prevents us from passing a string with a **different size**.

---

- Passing by **value** however *does decay* $\to$ convert *raw array type* to the **corresponding raw pointer**.
- This is actually effective therefore the constructor should be **taking in by value**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220819005550437.png)

- Prefer to use `std::move(elem)` to avoid unnecessary copying into vector.

### Deduction Guides

- Provide a **deduction guide** to prevent dealing with **raw pointers**.

```c++
// pass a string literal or C string, instantiate using a std::string
Stack(char const *) -> Stack<std::string>;
```

- This guide must be in the **namespace** of the *class definition*, often follows directly after.
- The type after `->` is the *guided type*.

```c++
Stack stringStack {"bottom"}; // deduce to std::string

Stack stringStack = "bottom"; // error, std::string deduced but not valid. 
```

- Cannot copy initialize to a constructor expecting a **std::string** with a **string**.

---

- Class template argument deduction copies 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220819012136875.png)

## Templatized Aggregates

- Aggregate classes may be templates.

```c++
template <typename T>
struct ValueWithComment {
    T value;
    std::string comment;
};
```

```c++
ValueWithComment<int> vc;
vc.value = 42;
vc.comment = "initial value";
```

- Since c++17 $\to$ define **deduction guides** for aggregate class templates.

```c++
ValueWithComment(char const*, char const*)
-> ValueWithComment<std::string>;
ValueWithComment vc2 = {"hello", "initial value"};
```

- Without **deduction guide** $\to$ init would **not be possible** has `ValueWithComment` has **no constructor** to *perform the deduction against*.

---

- `std::array<>` is an **aggregate** that is **parameterized** for *both the element type* and *size*.

# Nontype template parameters

- template parameters **do not have to be types**.
- Define code for where a **certain detail** remains open until the *code is used*.
- This detail is a **value** instead of a **type.**

---

## Nontype class template parameters

- Use a **fixed size array** for a stack by using a **fixed size array** for the elements.
- Memory management **overhead**, whether by you or some **standard container**.
- Determining **stack size** is a *challenging task*.
- Let the **user decide** the **stack size**.

```c++
#include <array>
#include <cassert>
template<typename T, std::size_t Maxsize>
class Stack {
    private:
        std::array<T,Maxsize> elems; // elements
        std::size_t numElems; // current number of elements
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220819185251100.png)

- The **new second template parameter**, `MaxSize` is type int, specifies the **size** of the **internal array of stack elements**.

```c++
template <typename T, std::size_t MaxSize>
class Stack {
    private:
    	std::array<T,MaxSize> elems; // elements
    	...
}
```

- Additionally used in push to check the **stack is full**

```c++
template <typename T, std::size_t MaxSize>
void Stack<T, MaxSize>::push (const T& elem){
    assert(numElems < MaxSize);
    elems[numElems] = elem; // append element
    ++numElems; // increment number of elements.
}
```

- To use this **class template** $\to$ must specify both the **element type** and **maximum size**.

```c++
#include "stacknontype.hpp"
#include <iostream>
#include <string>

int main()
{
    Stack<int,20> int20Stack; // stack of up to 20 ints
    Stack<int,40> int40Stack; // stack of up to 40 ints
    Stack<std::string,40> stringStack; // stack of up to 40 strings
    
    // manipulate stack of up to 20 ints
    int20Stack.push(7);
    std::cout << int20Stack.top() << '\n';
    int20Stack.pop();
    
    // manipulate stack of up to 40 strings
    stringStack.push("hello");
    std::cout << stringStack.top() << '\n';
    stringStack.pop();
}
```

- They are completely different types 20 vs 40 stack, no implicit / explicit conversions.

```c++
template<typename T = int, std::size_t MaxSize = 100>
class Stack {
    ...
}
```

- Design choice good to not actually set **default parameters** which are not really justifiable in any reasonable way.

## Nontype function template parameters

```c++
template<int Val, typename T>
T addValue (T x)
{
	return x + Val;
}
```

- Useful if **functions** or *operations* are used as *parameters*.
- Use a c++ standard library $\to$ you can **pass an instantiation** of this function template to add a **value** to *each element of a collection*

```c++
std::transform (source.begin(), source.end(), // start and end of source
    dest.begin(), // start of destination
    addValue<5,int>); // operation
```

- The *last argument* just **instantiates** the *function template* `addValue<>()` to add `5` to a passed `int` value.
- Must specify the **argument** `int` for the *template parameter* `T` of `addValue<>()`.
- Deduction works for **immediate calls** and `std::transform()` need a **complete type** to *deduce* the *type* of its **fourth parameter**.

---

- No support to **substitute / deduce** only *some template parameters* and the *see* what could fit and **deduce remaining parameters**.
- Specify a **template parameter** is deduced from the **previous parameter**

```c++
template <auto Val, typename T =  decltype(Val)>
T foo();
```

- Ensure passed value has the same type as the passed tpe

```c++
template <typename T, T val = T {}>
T bar();
```

## Restrictions for Nontype template parameters

- Note that *nontype* template parameters carry **some restrictions**. 
  1. Constant integral values including enumerations
  2. pointers to objects / functions / members
  3. lvalue references to objects or functions
  4. std::nullptr_t

```c++

template<double VAT> // ERROR: floating-point values are not
double process (double v) // allowed as template parameters
{
	return v * VAT;
}
template<std::string name> // ERROR: class-type objects are not
class MyClass { // allowed as template parameters
	...
};
```

- passing template arguments to **pointers / references** $\to$ objects must **not be string literals** / **temps** / **data members** / **other sub objects**.

---

- Restrictions relaxed in each C++ before 17

---

- Further constraints
  1. C++11 > objects had to have **external linkage**
  2. C++14 > objects also had to have **external / internal linkage**

---

``` c++
template <char const* name>
class MyClasss {
    ...
};

MyClass<"hello"> x; // Error, string literal "hello" not allowed
```

```c++
extern char const so3[] = "hi"; // external linkage
char const s11[] = "hi"; // internal linkage

int main(){
    Message<so3> mo3; // ok all versions
    Messsage<s11> m11; // ok since c++11
    
    static char const s17[] = "hi"; // no linkage
    
    Message<s17> m17; // Ok Since C++17.
}
```

- Each case $\to$ constant character array init with "hello" and this is used as a template parameter declared with `char const*`  

### Avoiding invalid expressions

- Arguments for **nontype template parameters** may be any compile time expressions

```c++
template<int I, bool B>
class C;
...
C<sizeof(int) + 4, sizeof(int)==4> c;
```

- If `operator>` used in the expression, must have parentheses

```c++
C<42, (sizeof(int) > 4)> c;
```

## Template Parameter Type `auto`

- Define NTTP to *generically accept* any type that is allowed for a **non type parameter**.
- Provide even more generics for the **stack class** with **fixed size**

```c++
#include <array>
#include <cassert>

template <typename T, auto Maxsize>
class Stack {
 public:
  using size_type = decltype(Maxsize);

 private:
  std::array<T, Maxsize> elems;  // elements
  size_type numElems;            // current number of elements
 public:
  Stack();                   // constructor
  void push(T const& elem);  // push element
  void pop();                // pop element
  T const& top() const;      // return top element
  bool empty() const {       // return whether the stack is empty
    return numElems == 0;
  }
  size_type size() const {  // return current number of elements
    return numElems;
  }
};

// constructor
template <typename T, auto Maxsize>
Stack<T, Maxsize>::Stack()
    : numElems(0)  // start with no elements
{
  // nothing else to do
}

template <typename T, auto Maxsize>
void Stack<T, Maxsize>::push(T const& elem) {
  assert(numElems < Maxsize);
  elems[numElems] = elem;  // append element
  ++numElems;              // increment number of elements
}

template <typename T, auto Maxsize>
void Stack<T, Maxsize>::pop() {
  assert(!elems.empty());
  --numElems;  // decrement number of elements
}

template <typename T, auto Maxsize>
T const& Stack<T, Maxsize>::top() const {
  assert(!elems.empty());
  return elems[numElems - 1];  // return last element
}
```

- By defining

```c++
template<typename T, auto MaxSize>
class Stack {
    ...
};
```

- Using placeholder `auto` $\to$ define a `MaxSize` to be a **value** off a **type not specified yet**.

- Internally use the **value and type**

```c++
std::array<T,MaxSize> elems; // elements
using size_type = decltype(MaxSize);

// use as return type for size() member function
size_type size() const {
    return numElems;
}

// use auto since c++14

auto size() const {
    return numElems;
}
```

```c++
#include "stackauto.hpp"
#include <iostream>
#include <string>

int main() {
  Stack<int, 20u> int20Stack;          // stack of up to 20 ints
  Stack<std::string, 40> stringStack;  // stack of up to 40 strings

  // manipulate stack of up to 20 ints
  int20Stack.push(7);
  std::cout << int20Stack.top() << '\n';
  auto size1 = int20Stack.size();

  // manipulate stack of up to 40 strings
  stringStack.push("hello");
  std::cout << stringStack.top() << '\n';
  auto size2 = stringStack.size();

  if (!std::is_same_v<decltype(size1), decltype(size2)>) {
    std::cout << "size types differ" << '\n';
  }
}
```

- Thus having different return types for each of the defined size() methods.
- C++17 gives us _v suffix for values of **traits** `std::is_same_v` 

# 9 - Templates in practice

