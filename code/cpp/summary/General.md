# General C++

## Basics

- **Statements**: Instructions in a program, usually ending with a semicolon.
- **Identifiers**: Names given to functions, objects, types, etc.
- **Initialization vs. Assignment**: Prefer brace initialization and initialization over assignment.
- **Variable Declaration**: Define and initialize variables separately for clarity.
- **Uninitialized Variables**: Variables without assigned values can lead to undefined behaviour.
- **Keywords**: Reserved names in C++ with special meanings, not usable as variable names.
- **Literal Constants**: Fixed values directly inserted into source code, like numbers or text strings.
- **Operations and Operators**: Processes with operands and operators to produce output. Types: unary, binary, ternary, nullary.
- **Expressions and Expression Statements**: Combinations of literals, variables, and operators evaluated to produce a single value. Expression statements end with a semicolon.
- **Parameters**: Variables defined in a function's declaration
- **Arguments**: Specific values passed from the caller to the function
- **Pass by value**: Process of copying an argument into a parameter
- **Local variables**: Defined within a function with limited lifetime and scope
- **Scope**: Determined at compile time, affecting variable access
- **Forward Declaration**: Prototype informing compiler about an identifier without full definition
- **Definitions**: Implement or instantiate an identifier, also declarations
- **Declarations**: Inform compiler about the existence of an identifier
- **Program Structure**: Complex programs often split into multiple files
- **Namespaces**: Help avoid naming collisions, like `std`
- **Preprocessor Directives**: Modify code before compilation, start with `#`
- **Macros**: Define rules for text replacement
- **Header Files**: Propagate declarations to code files
- **#include Directive**: Use angled brackets for system headers, double quotes for user-defined headers
- **Header Guards**: Prevent multiple inclusions in the same code file, allow across different files
- **Bit**: Smallest unit of memory
- **Byte**: Smallest addressable memory unit, typically 8 bits
- **Data Types**: Instruct compiler on interpreting memory contents, include various fundamental types
- **Void**: Indicates no type, used in non-returning functions
- **Memory Usage by Data Types**: Varies, `sizeof` operator returns size in bytes
- **Signed Integers**: Store positive, negative, zero whole numbers
- **Unsigned Integers**: Store non-negative numbers, used in bit-level manipulation
- **Fixed-width Integers**: Guaranteed sizes, may not be available on all architectures
- **size_t**: Unsigned integral type for object sizes or lengths
- **Floating Point Numbers**: Represent real numbers, including fractions
- **Precision in Floating Points**: Number of significant digits without information loss
- **Rounding Errors**: Can occur in floating points due to precision limits
- **Boolean Type**: Stores true or false values
- **if Statements**: Execute code based on a boolean condition
- **else Statements**: Execute code when `if` condition is false
- **Char Type**: Stores ASCII characters, distinguish from numbers
- **Type Conversion**: Use `static_cast<Type>(x)` for converting `x` to `Type`
- **Scientific Notation**: Represents long numbers concisely, supported in C++ for floating points

## Constants and Strings

- **Constant**: A value that remains unchanged during program execution, with C++ supporting named constants and literals.
- **Named Constants**: Constant values associated with identifiers, including constant variables and object-like macros.
- **Literal Constants**: Constant values not associated with identifiers.
- **Constant Variable**: Created with the `const` keyword, cannot have its value changed and must be initialized at declaration.
- **Type Qualifiers**: Keywords modifying type behavior, with `const` and `volatile` being the only ones supported in C++23.
- **Constant Expressions**: Expressions that can be evaluated at compile-time, distinct from runtime expressions.
- **Compile-Time vs. Runtime Constants**: Compile-time constants are known during compilation, while runtime constants are determined during execution.
- **`constexpr` Variables**: Must be initialized with a constant expression and cannot have function parameters as `constexpr`.
- **Literals**: Values inserted directly into the code, with types alterable using literal suffixes.
- **Magic Numbers**: Literals with unclear meaning or potentially changeable values, advised to be avoided in favor of symbolic constants.
- **Numeral Systems**: Includes decimal, binary, octal, and hexadecimal, all supported in C++.
- **Conditional Operator (`?:`)**: A ternary operator that evaluates one of two expressions based on a condition.
- **Inline Expansion**: A process where a function call is replaced with the called function’s code, applicable to inline functions and variables.
- **`constexpr` Function**: A function whose return value may be computed at compile-time.
- **`consteval` Function**: Must be evaluated at compile-time.
- **String Handling**: Involves C-style strings, `std::string` for safe text handling, and `std::string_view` for read-only access to strings.

## Operators

## Scope, Duration and Linkage

## Control Flow and Error Handling

## References & Pointers

## Type Conversion and Function Overloading

## Enums and Structs

## Classes 

> - OOP
> - Virtu

## Functions

> - Lambdas
> - Function Pointers
> - Ellipsis
> - Stack & Heap

## Templates

## Exceptions

## I/O

## Bit Manipulation