# Template Metaprogramming Type Traits

https://youtu.be/tiAVWcjIF6o?t=905

- Programs treat programs with data
- Could be other programs or itself
- Could be at *compile* *time* or at *runtime*.

---

- Required for advanced use cases
- Writing libraries
- tools and idioms are well developed and not boost / STL limited.
- All C++ programmers should understand the basics.
- C++ 20 $\to$ concepts and independent requires expressions.

---

## Meta-functions

```C++
/* traditional functions have zero+ parameters and return a value (or void) */
void do_something();
int do_something_else(int, char const *);
```

```c++

/* mechanism for returning a value from a function is return*/
int do_something_else(int, char const *){
    return 42;
}

/* compiler can enforce return values and syntax.*/
```

- The meta functions are **class / structs**
- These are **not part of the language** and thus have **no formal language support**
  - Exist as an *idiomatic* use of **existing language features**
- Their use is **not enforced by the language**.
  - This becomes **dictated** by *conventions*.
    - Created standard common conventions.

---

- They are classes with $zero+$ template parameter and $zero+$ return type and values
  - Convention is that a **metafunction** should *return one thing* such as a **regular thing**.
- Convention was **developed over time**
  - Thus plenty of **existing examples** that *do not follow this convention*.
- More **modern metafunctions ** do *follow this convention*.

---

### Return from a meta function

- Expose some public value *value*

```c++
template <typename T>
struct TheAnswer {
    static constexpr int value = 42;
}
```

- Alternatively type based, we expose a public *type*

```c++
template <typename T>
struct Echo {
    using type = T;
}
```

### Value metafunction

- Simple regular function $\to$ identity

```c++
int int_identity(int x){
    return x;
}

assert(42 == int_identity(42));
```

- Simple metafunction $\to$ identity

```c++
template <int X>
struct IntIdentity {
    static constexpr int value = x;
}
static_assert(42 == IntIdentity<42>::value);
```

### Generic Identity function

```c++
template <typename T>
T identity(T x){
    return x;
} 

// return type is int
assert(42 == identity(42));

// return type will be unsigned long long
assert(42ull == identity(42ull));
```

### Generic identity metafunction

```c++

/* feed it the value of in*/
template <typename T, T Value>
struct ValueIdentity {
    static constexpr T value = Value;   
}

// Type of value will be int
static_assert(42 == ValueIdentity<int, 42>::value);

// Type of value is unsigned long long
static_assert(ValueIdentity<unsigned long long, 42ull>::value == 42ull);
```

