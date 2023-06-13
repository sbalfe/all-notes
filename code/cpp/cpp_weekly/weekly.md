# 178 - important bits of C++14

- Added `auto` return type deduction 

```c++
template <typename T>
auto count_things(const T &vec, int value){
    const auto count = std::count(begin(vec), end(vec), value);
    return count;
}
```

- Lambdas are now *generic*

```c++
template<typename T>
void count_things_less_than_3(const T&vec , int value){
    const auto count = std::count(begin(vec), end(vec),
        [](const auto i){return i < 3;}
    );
}
```

- Generalised capture expression

```c++
template<typename T>
void count_things_less_than_3(const T&vec , int value){
    const auto count = std::count(begin(vec), end(vec),
                                  
        /*capture the value based on a lambda evaluation to obtain its variable by instant invocation*/
        [value = [](){return 3;}]()(const auto i){return i < value;}
    );
}
```

- make unique added
- NEW and DELETE are no longer added.

---

# 213 - CTRE - Compile Time Regular Expressions

```c++
#include <ctre.hpp>
#include <string_view>

bool is_error_string(std::string_view sv){
    return ctre::match<".*ERROR.*">(sv)
}
```

# 12 - std::any c++17

- Item that can store anything.

```c++
#include <any>
#include <vector>
#include <string>
#include <iostream>

struct S {
    S(const S &s) = default;
    S() = default;
};


int main()
{
    std::vector<std::any> v{5, 3.4, std::string{"hello world"}};

    std::cout << v.size() << std::endl;
    std::cout << v[1].type().name() << '\n';
    return 0;
}
```

# 17 - std::invoke c++ 17

```c++
#include <functional>
#include <iostream>

int do_something(const int i){
    return 5 + i;
}

struct S {

    const int j {5};

    int do_something(const int i){
        return j + i;
    }

    int do_something_2(const int i){
        return j * i;
    }

};

int main()
{
    std::cout << std::invoke(&do_something, 5) << '\n';

    S s;

    std::cout << s.do_something(3) << '\n';

    auto fp = &S::do_something;

    /* function pointer call fp2 that points to something in scope of S
        taking a parameter of S
    */
    int (S::*fp2)(int) = nullptr;

    /* conditionally bind the function pointer to something */
    if (true){
        fp2 = &S::do_something_2;
    } else {
        fp2 = &S::do_something;
    }

    std::cout << (s.*fp2)(3) << '\n';
    std::cout << (s.*fp)(2) << '\n';

    /* use std::invoke to simplify this calling process */

    std::cout << std::invoke(&S::do_something, s, 10) << '\n';

    /* access member data too */
    std::cout << std::invoke(&S::j, s) << '\n';
}
```

# 18 - constexpr if c++ 17

```c++
#include <functional>
#include <iostream>
#include <type_traits>
#include <limits>

/* select branch at compiletime using constexpr if based on type traits*/
template<typename T>
constexpr bool is_both(){

    if constexpr(std::is_integral<T>::value && !std::is_same<bool, T>::value){

        if constexpr(std::numeric_limits<T>::min() < 1000){
            return true;
        }
    }
    return false;

}

/* selects the body at compile time removing the lines for other bits of the if statements*/
template<typename T>
auto print_type_info(const T& t){
    if constexpr(is_both<T>()){
        return t+1;
    } else {
        return t+0.1;
    }
}

int main()
{
    std::cout << print_type_info(5) << '\n';
    std::cout << print_type_info(2.5) << '\n';
}
```

# 20 - fold expressions c++17

```c++
#include <iostream>
#include <vector>
#include <string>

template<typename ...T>
auto sum(T ...t){
    return (t + ...);
}

template<typename ...T>
auto div(T ...t){
    const int n {5};
    return (... / t);
}

/* use .. to count number of items passed and ... to fold a plus operand along each of the inputs*/
template<typename ...T>
auto avg(T ...t){
    return (t + ...) / sizeof...(T);
}

int main()
{
    std::cout << avg(3,3,3) << std::endl;
}
```

# 87 - std::optional

https://www.youtube.com/watch?v=PiaZkNp_fIM

```c++
#include <optional>

int main(){
    std::optional<int> o {13};
    auto val = o.value() /* bad access potential from using this */
    return o.value_or(2) /* return value or the contained value here*/
}
```

```c++
#include <optional>
#include <string>

struct S {
    S(const S&) = delete;
    S(S&&) = delete;
    S() = default;
};

int main(){

    std::optional<S> o; 
    o.emplace(); /* constructs S in this the std::optional */
}
```

