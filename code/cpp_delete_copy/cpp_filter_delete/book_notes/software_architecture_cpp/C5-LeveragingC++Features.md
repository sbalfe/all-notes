# Designing APIs

## RAII

```c++
#pragma once

struct Resource;

// C API
Resource *acquireResource() {}
void releaseResource(Resource *resource) {}

// C++ API - managing special resource so use a custom deleter.
using ResourceRaii = std::unique_ptr<Resource, decltype(&releaseResource)>;
ResourceRaii acquireResourceRaii() {}
```

- Use for any resource
  1. locks
  2. file handles
  3. database connections

## Specifying the interfaces of containers in C++

- `std::array` amongst other containers contains alias for various common types used in contaiers,.

```c++
template<class T, size_t N>
struct array{
    using reference = T&;
    using const_reference = const T&;
    ...
}
```

- Using these ensures **type traits** are in place for operating with various templated standard library code to ensure they are **compatible**.
- API users must work around this and use a **different class** *entirely*.
- Used for **function parameters** and **class member fields** also not just fore templates.
  - Allocator $\to$ depends on specify **type aliases** being present.

---

- `std::array` has no definition for 
  1. constructor
  2. move / copy  constructors
  3. assignment operators
  4. destructors.
- A **non defaulted constructor** actually prevents certain **compiler optimizations**.

---

```c++
constexpr void fill(const T& u);
constexpr void swap(array<T,N>&) noexcept(is_nothrow_swappable<T&>)
```

- Write custom `swap` detected via **argument dependent lookup** which may eventually call default `std::swap`. 
- also `noexcept` meaning its ***conditonally noexcept*** depending on the stored type being swapped **without throwing exceptions**.
  - Achieves **strong exception safety**.

---

- Providing **iterators** is important to be compatible with **ranged based for loops**.
- Can be as simple as **simple pointer** if the memory is **contiguous**.
- Provision of **const iterators** enables are class to be **immutable**

---

- Provide methods to **inspect / modify** the containers data.

```c++
constexpr T* data () noexcept;
...
```

- Array must be made `constexpr` as the type and size of container is known at **compile time**.

---

- Can also view **deduction guides** in C++17 

```c++
template<class T, class... U>
array(T, U...) -> array<T, 1 + sizeof...(U)>
```

- This enables the **type** to be **deduced** and the **number of arguments** determines the **instantiation** of array to be called

```c++
auto arr = std::array {1,2,3} // deduced as array<int, 3>
```

- This is called **class template argument deduction**

- More useful for **complex types**

```c++
auto legCount = std::unordered_map{ std::pair{"cat", 4}, {"human", 2},{"mushroom", 1} };
```

- require `std::pair` for the **first argument** where a **type deduction** guide is created.

---

## Using Pointers in Interfaces

- 
