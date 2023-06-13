## Move semantics

```c++
std::string func1(const std::string& str){ /* implicit &memory_location if being assigned passed in as a param*/
    std::string tmp{str};
    return tmp; /* if func1() is being stored then RVO */
}

void wrap1(const std::string&){
    func1(arg);
}

template<typename T>
std::string func2(T&& arg){
    std::string tmp{std::forward<T>(arg)};
    return tmp;
}

template<typename T>
void wrap2(T&& arg){
    func2(std::forward<T>(arg)); // forward move semantics to another R value reference function so library makers speed up by distinguishing what can be copied and moved they can avoid spurios copies.
}

int main(){
    /* temp implictly is copied into a std::string temporary to bind to the const std::string&
    	copied again when constructed in func1
    */
    std::string data = "value";
    wrap1("temp");
    
    /* temp forwarded to the construction in func1 keeping it a temp so it only gets copied when
    	the std::string is being made
    */
    wrap2("temp");
}


```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221013141601456.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221013141938210.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221013142126186.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221013142831904.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221013144154022.png)

```C++
#include <iostream>

struct S {
    S() { std::cout << "Default construction\n"; }
    S(const char *) { std::cout << "const char * construction\n"; }
    S(const S&) { std::cout << "Copy construction\n"; }
    S(int &&) { std::cout << "Move construction\n"; }
    S& operator=(const S&) { std::cout << "Copy assignment\n"; return *this; }
    S& operator=(S&&) { std::cout << "Move assignment\n"; return *this; }
    ~S() { std::cout << "Destruction\n"; }
};

S func1(const S& arg) {
    std::cout << "Call func1\n";
    S test(arg);
    std::cout << "return func1\n";
    return test;
}

void wrap1(S&& arg) {
    std::cout << "Call wrap1\n";
    func1(arg);
    std::cout << "Return wrap1\n";
}

int main(){
  S test{}; // default construction
  wrap1(std::move(test)); // interpret as R value calls wrap 1 overload but semantically the same as const S&
  wrap1(S(1)); // move construction and then calls wrap1 
}
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20221013145123792.png)

```c++
struct S {
    std::unique_ptr<int> val {std::make_unique<int>(2)};
};

[[nodiscard]] const std::unique_ptr<int>& func(const S& v){
    return v.val; // if assigned like auto getPtr = func(svalue), it copys which is blocked on unique pointer
    return std::move(v.val); // this is correct and change return type to std::unique_ptr<int> to simply move ownership into the assigned value, auto getPtr = func(svalue), get Ptr now holds ownership with no copying.
}
```

