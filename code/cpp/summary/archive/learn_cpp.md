# Learncpp log

- [x] Chapter 1

- [x] Chapter 2

- [x] Chapter 3 - Debugging C++ programs - ==Could review==

- [ ] Chapter 4 - Fundamental data types - ==Complete this==

  - [x] [void](https://www.learncpp.com/cpp-tutorial/void/) 

    - Use an empty parameter list instead of `void`.

  - [x] [Object sizes and the sizeof operator](https://www.learncpp.com/cpp-tutorial/object-sizes-and-the-sizeof-operator/)

    - Wide strings 
    - C++ does not define exact sizes of data types, it sets only *minimum sizes*.

  - [x] [Fixed width integers and size_t](https://www.learncpp.com/cpp-tutorial/fixed-width-integers-and-size-t/)

    - Fixed width integers are to enable a consistent size across various architectures, they include:

    - Prefer these over `short` and `long`.

      ```cpp
       std::int8_t std::uint8_t, std::int16_t, std::uint16_t, std::int32_t, std::uint32_t, std::int64_t, and std::uint64_t
      ```

    - 8 bit fixed width integers `std::int8_t` and `std::uint8_t` should be *avoided* because they are often treated like the `char` datatype. We therefore prefer the 16 bit alternative.

    - Use `int` when the size of our integral is not critical. It is at least 2-bytes therefore should be adequate.

    - `std::size_t` is to represent the size or length of objects. It is returned by the `sizeof` operator.

    - Avoid using the `fast` and `least` width types

    - Avoid compiler specific fixed width integers.

  - [ ] [Signed integers](https://www.learncpp.com/cpp-tutorial/signed-integers/)

  - [ ] [Unsigned integers and why to avoid them]([Unsigned integers, and why to avoid them](https://www.learncpp.com/cpp-tutorial/unsigned-integers-and-why-to-avoid-them/))

  - [ ] [Floating point numbers](https://www.learncpp.com/cpp-tutorial/floating-point-numbers/)

- [ ] Chapter 5 - Constants and Strings 
  - [x] [Constant variables (named constants)](https://www.learncpp.com/cpp-tutorial/constant-variables-named-constants/)
    - *Literal constants* - are constant values that are not associated with an identifier (aka *symbolic constants*)
    - *Named constants* - are constant values that are associated with an identifier.
    - Constants may be:
      - *Macros*
      - *Constant variables*
      - *Enumerated constants*
    - *Type qualifiers* - constant and volatile (cv - qualified)
    - `Volatile` - tell the compiler an object may have its valued changed at any time. Disables certain compiler optimisations.
  - [x] [Inline functions and variables](https://www.learncpp.com/cpp-tutorial/inline-functions-and-variables/)
    - Do not use `inline` to request inline expansion i.e for performance.
    - Compiler needs to be able to see the full definition of an inline function or variable whenever its used
    - Avoid `inline` unless you need it such as definind variables in headers for ODR or functions in headers.
  - [ ] [constexpr and consteval functions](https://www.learncpp.com/cpp-tutorial/constexpr-and-consteval-functions/)
  
- [ ] Chapter 6 - Operators

  - [ ] [Relational operators and floating point comparisons](https://www.learncpp.com/cpp-tutorial/relational-operators-and-floating-point-comparisons/)

- [ ] Chapter O - Bit manipulation ==WORTH REVIEWING==

- [x] Chapter 7 - Linkage duration and scope

- [ ] Chapter 8 - Control Flow and Error Handling

  - [ ] [std::cin and handling invalid input](https://www.learncpp.com/cpp-tutorial/stdcin-and-handling-invalid-input/)

- [ ] Chapter 10 - Type conversion and function overloading - try and find a resource / summary page -https://www.learncpp.com/cpp-tutorial/implicit-type-conversion/

  - **Implicit Type Conversion (Coercion)**: This occurs when the compiler automatically converts one type of data to another. For example, converting an integer to a float when these types are mixed in expressions.

  - **Floating Point and Integral Promotion**: This refers to the process where smaller integer types (like `char` and `short`) are converted to `int` or `unsigned int`, and `float` is promoted to `double` when used in expressions.

  - **Numeric Conversions**: These are conversions between different numeric types, like converting from `int` to `float`, or `long` to `double`. These can be implicit or explicit.

  - **Narrowing Conversions**: This occurs when a larger type is converted to a smaller type, like `double` to `float` or `long` to `int`. This can lead to loss of data and is generally avoided or done explicitly.

  - **Arithmetic Conversions**: These are standard conversions that are applied to yield a common type for binary arithmetic operations. For example, if an `int` and a `long` are operated on, both might be converted to `long`.

  - **Explicit Type Conversions (Casting)**: This is when you manually convert from one type to another. For example, using `(int)myDouble` or `static_cast<int>(myDouble)` in C++ to convert a `double` to an `int`.

  - **Function Overload Differentiation**: This is about how a programming language chooses the right function to call when multiple overloaded functions exist. It is based on the number, type, and order of parameters.

  - **Function Overload Resolution**: This is a more specific part of function overload differentiation. The compiler uses a set of rules to find the best match for the function call from a set of overloaded functions. The rules consider aspects like the exactness of the match, promotions, and conversions needed

- [ ] Chapter 12 - [References and pointers](https://www.learncpp.com/cpp-tutorial/type-deduction-with-pointers-references-and-const/)

  - [ ] [Type deduction with pointers, references and const](https://www.learncpp.com/cpp-tutorial/type-deduction-with-pointers-references-and-const/)

- [ ] Chapter 13 - [Compound Types: Enums and Structs](https://www.learncpp.com/cpp-tutorial/introduction-to-program-defined-user-defined-types/)

  - [x] [Class init and copy elision](https://www.learncpp.com/cpp-tutorial/class-initialization-and-copy-elision/)

  - [ ] [CTAD](https://www.learncpp.com/cpp-tutorial/class-template-argument-deduction-ctad-and-deduction-guides/) 

- [ ] Chapter 14 / 15 - Classes

  - [Members function returning references to data members](https://www.learncpp.com/cpp-tutorial/member-functions-returning-references-to-data-members/)

    - ```cpp
      #include <iostream>
      #include <string>
      #include <string_view>
      
      class Employee {
          std::string m_name{};
      
      public:
          // Setter function
          void setName(std::string_view name) { m_name = name; }
      
          // Getter function returning a const reference to the data member
          const std::string& getName() const { return m_name; }
      
          // A member function that may return by reference should return a const reference
          // to avoid dangling references and unintended modifications
      };
      
      // Function demonstrating returning a reference from non-member function
      const std::string& firstAlphabetical(const std::string& a, const std::string& b) {
          return (a < b) ? a : b;
      }
      
      // Function that returns an Employee object by value (rvalue)
      Employee createEmployee(std::string_view name) {
          Employee e;
          e.setName(name);
          return e;
      }
      
      int main() {
          Employee joe{};
          joe.setName("Joe");
      
          // Safe usage of returning reference from member function
          std::cout << joe.getName() << std::endl; // Safe: 'joe' is an lvalue
      
          // Example of using return value of a function returning by reference immediately
          std::cout << firstAlphabetical("Hello", "World") << std::endl;
      
          // Correct way to handle rvalue references
          std::string value = createEmployee("Hans").getName(); // Safe: copies the string
          std::cout << value << std::endl;
      
          // Wrong way: storing a reference to a member of a temporary (rvalue)
          // const std::string& ref = createEmployee("Garbo").getName(); // Unsafe: 'ref' becomes dangling
      
          return 0;
      }
      
      // Key Points:
      // 1. Do not return non-const references to private data members.
      // 2. Use const references to return expensive-to-copy types.
      // 3. Avoid returning references to local variables, temporaries, or function arguments not passed by reference.
      // 4. Ensure the lifetime of the returned reference is managed correctly, especially with rvalues.
      // 5. Prefer explicit return types over 'auto' for documentation clarity.
      
      ```

- [ ] Chapter 16  - ==Review std::vector in detail==   [learn cpp std::vector](https://www.learncpp.com/cpp-tutorial/introduction-to-containers-and-arrays/)

  - [ ] GPT summary
    1. **Containers**: Vectors are a type of container, which are data structures that store a collection of elements. They are homogeneous, meaning all elements are of the same type.
    2. **Array Types in C++**: There are three primary array types: C-style arrays, `std::vector`, and `std::array`. `std::vector` is a dynamic array that allows size modifications and offers direct access to elements.
    3. **Vector Initialization**: Vectors can be initialized using a list constructor with initializer lists. You can initialize vectors with a specific number of elements or a list of values.
    4. **Element Access**: Elements in an array can be accessed using the subscript operator (`operator[]`), which does not perform bounds checking. The `at()` method does bounds checking and throws an exception if out of range.
    5. **Size and Capacity**: Vectors have a size (number of elements currently held) and capacity (space allocated for elements). `size_type` is used for representing sizes and indices in vectors.
    6. **Passing Vectors**: Vectors can be passed by value or by (const) reference to functions. Passing by value creates a copy, so reference is preferred to avoid expensive copies.
    7. **Move Semantics**: Vectors support move semantics, allowing efficient transfer of resources from one vector to another.
    8. **Traversal**: Traversal or iteration over vectors can be done using loops. Range-based for loops are preferred for their simplicity and readability.
    9. **Fixed-size vs Dynamic Arrays**: `std::vector` is a dynamic array, meaning its size can change after instantiation. Fixed-size arrays have a constant size.
    10. **Resizing Vectors**: Vectors can be resized using the `resize()` function, and their capacity can be managed with the `reserve()` and `shrink_to_fit()` methods.
    11. **Stack Operations**: Vectors support stack-like operations (`push_back()`, `emplace_back()`) for adding elements. `emplace_back()` can be more efficient than `push_back()` in certain scenarios.
    12. **Specialization for bool**: `std::vector<bool>` is a specialized implementation that is not fully compatible with other vector types and should generally be avoided.
    13. **Encouragement**: The chapter acknowledges the complexity of these concepts and congratulates the reader on their progress.

- [ ] Chapter 17- Fixed arrays: std::array & C-style array - ==REVIEW and feed into main summary== 

  - GPT code summary:

    ```cpp
    // Demonstrating brace elision with std::array
    void braceElisionExample() {
        std::array<int, 3> array1 = {1, 2, 3}; // Single braces for scalar values
        std::array<std::array<int, 2>, 2> array2 = {{{1, 2}, {3, 4}}}; // Nested braces can be omitted
    }
    
    // Demonstrating std::reference_wrapper
    void referenceWrapperExample() {
        int a = 10, b = 20;
        std::array<std::reference_wrapper<int>, 2> refArray = {a, b};
    
        refArray[0] = b; // Operator= reseats the reference
        int& ref = refArray[1].get(); // get() returns a reference
        ref = 30; // Modifies 'b'
    }
    
    // Demonstrating static_assert with CTAD
    void staticAssertWithCTAD() {
        std::array array = {1, 2, 3};
        static_assert(array.size() == 3, "Array must have 3 elements");
    }
    
    // Using C-style arrays
    void cStyleArrayExample() {
        int cArray[] = {1, 2, 3, 4, 5}; // Length is deduced
        constexpr size_t length = std::size(cArray); // C++17: Get length
    
        // Accessing elements
        for (size_t i = 0; i < length; ++i) {
            std::cout << cArray[i] << " "; // Using subscript operator
        }
        std::cout << "\n";
    }
    
    // Demonstrating array decay and pointer arithmetic
    void arrayDecayAndPointerArithmetic() {
        int array[] = {10, 20, 30};
        int* ptr = array; // Array decays to pointer
    
        std::cout << *(ptr + 1) << "\n"; // Pointer arithmetic
    }
    
    // Working with multidimensional arrays
    void multidimensionalArrayExample() {
        int twoDimArray[2][3] = {{1, 2, 3}, {4, 5, 6}}; // 2D array
    
        // Accessing elements
        for (int i = 0; i < 2; ++i) {
            for (int j = 0; j < 3; ++j) {
                std::cout << twoDimArray[i][j] << " ";
            }
            std::cout << "\n";
        }
    }
    
    // Using std::mdspan in C++23
    void mdspanExample() {
        int data[] = {1, 2, 3, 4, 5, 6};
        std::mdspan<int, std::dynamic_extent, std::dynamic_extent> span(data, 2, 3); // 2x3 mdspan
    
        // Accessing elements
        for (size_t i = 0; i < span.extent(0); ++i) {
            for (size_t j = 0; j < span.extent(1); ++j) {
                std::cout << span(i, j) << " ";
            }
            std::cout << "\n";
        }
    }
    
    int main() {
        braceElisionExample();
        referenceWrapperExample();
        staticAssertWithCTAD();
        cStyleArrayExample();
        arrayDecayAndPointerArithmetic();
        multidimensionalArrayExample();
        mdspanExample();
    
        return 0;
    }
    ```

    ```cpp
    // Fixed-size array using std::array
    template<typename T, std::size_t N>
    void useFixedSizeArray() {
        std::array<T, N> fixedArray; // Aggregate initialization
    
        // Access elements
        for (std::size_t i = 0; i < fixedArray.size(); ++i) {
            fixedArray[i] = T{}; // No bounds checking
        }
    }
    
    // Dynamic array using std::vector
    template<typename T>
    void useDynamicArray(std::size_t size) {
        std::vector<T> dynamicArray(size);
    
        // Resize at runtime
        dynamicArray.resize(size + 10);
    }
    
    // Function template to handle std::array of any type and size
    template <typename T, std::size_t N>
    void processArray(const std::array<T, N>& arr) {
        for (const auto& element : arr) {
            std::cout << element << " ";
        }
        std::cout << "\n";
    }
    
    // C-style array example
    template<typename T, std::size_t N>
    void useCStyleArray(const T (&arr)[N]) {
        // Access elements
        for (std::size_t i = 0; i < N; ++i) {
            std::cout << arr[i] << " ";
        }
        std::cout << "\n";
    }
    
    // std::reference_wrapper example
    template<typename T>
    void modifyThroughReference(std::reference_wrapper<T> ref) {
        ref.get() = T{}; // Modifies the referenced object
    }
    
    int main() {
        // Usage of fixed and dynamic arrays
        useFixedSizeArray<int, 5>();
        useDynamicArray<double>(10);
    
        // C-style array
        int cArray[] = {1, 2, 3, 4, 5};
        useCStyleArray(cArray);
    
        // Using reference wrapper
        int value = 10;
        std::reference_wrapper<int> refValue(value);
        modifyThroughReference(refValue);
    
        return 0;
    }
    ```

- [ ] Chapter 21 - [Operator overloading](https://www.learncpp.com/cpp-tutorial/introduction-to-operator-overloading/) - worth as a reference

- [ ] Chapter 25 - Virtual functions

  - [ ] [Object Slicing](https://www.learncpp.com/cpp-tutorial/object-slicing/)
  - [ ] [printing inherited classes using <<](https://www.learncpp.com/cpp-tutorial/printing-inherited-classes-using-operator/)

- [ ] Chatper 27 - Exceptions ==WORTH REVIEWING== 

- [ ] Chapter 28 - I/O ==WORTH REVIEWING== 


