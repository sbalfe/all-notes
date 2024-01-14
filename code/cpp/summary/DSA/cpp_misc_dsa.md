# Misc C++ learnt while doing DSA & leetcode

- [Why does std::for_each return a function](https://stackoverflow.com/questions/2048967/why-does-stdfor-eachfrom-to-function-return-function)

  - Example is counting how many times a function was called

  ```cpp
  struct Callable{
  
      Callable(): call_count_ {} {}
  
      void operator()(const auto& num1, const auto& num2){
          fmt::print("({},{})\n", num1, num2);
          ++call_count_;
  
      }
      int call_count_;
  };
  ```

- [fmt format strings](https://fmt.dev/4.1.0/syntax.html)

- [std::spanstream](https://medium.com/@simontoth/daily-bit-e-of-c-std-spanstream-c350cdc994a3#:~:text=The%20C%2B%2B23%20added,string%20views%20or%20string%20literals.)

- https://en.cppreference.com/w/cpp/ranges/view_counted
