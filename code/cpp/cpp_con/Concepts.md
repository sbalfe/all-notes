https://www.youtube.com/watch?v=_FoXWnrGuNU

---

# C++ 20 Templates : Concepts



![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220606201215611.png)

- Type trait if *no is*
- Concept if there *is* an **is** .

## Variadic template parameters of same type

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220606201428650.png)

- Do not want implicit conversions.
- Ensure each value is the same type.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220606201502893.png)

- Enable if prevents this from being enabled if some conditions are not meant such as the values not being the same 
- Enable if selects a return type based on the first bool are_same_v , then selects the first arg from the list of args to use.
- cannot instantiate a function if there is no return type.



- Concepts example

```c++
#include <iostream>

template<typename T, typename... Ts>
constexpr inline bool are_same_v = std::conjunction_v<std::is_same<T, Ts>...>;

template<typename... Args> requires are_same_v<Args...>
auto Add(Args&&... args) noexcept {
	return (... + args);
}

int main() {
	std::cout << Add(1, 2, 3, 10) << std::endl;
}
```



## Application Area for Concepts

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220610131204430.png)

## Requirements for our example concept of Add

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220610131504290.png)

## Requires expression

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220610131612442.png)

## Simple Requirement

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220610131931768.png)

## Nested Requirement

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220610132132845.png)

## Compound requirement

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220610132424604.png)

- Basically looks at some expression and evaluates the exception status and the type of the argument used, meeting the last two of the requirements listed above.
- -> checks return type using a concept same_as to declare if its valid or not.]
- overall {expression} noexcept -> concept/type checks if noexcept and a specific type is returned using concept

## Ad hoc constraints: requires requires

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220610133703812.png)

## Definition of a concept

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220610133802273.png)

- Concepts is a named set of requires clauses based on the incoming args passed to it.
- Use static asserts to test our concepts at compile time.

## Abbreviated function templates

```c++
// c++ 17 cringe version
template<typename T>
void DoLocked(T&& f){
    std::lock_guard lock {globalOsMutext};
    
    f();
}

// based c++ 20
void DoLocked(std::invocable auto&& f){
    std::lock_guard lock {globalsOsMutex};
    f();
}
```

## Error Messages

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220610135410948.png)

## More useful now

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220610143331336.png)
