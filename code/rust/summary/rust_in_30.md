# Rust in 30 minutes

- `let` introduces a variable binding

```rust
let x; // declares 'x'
x = 42; // assign 42 to 'x'
```

- This can be written as a single line

```rust
let x = 42;
```

- You can specify the variable type explicitly via `:`, thats a type annotation:

```rust
let x: i32; // i32 is unsigned 32-bit integer
x = 42;
// we have i8, i16, i32, i64, i127
// also u8, u16, u32, u64, u128 for unsigned
```

- This can be written as single line

```rust
let x: i32 = 42;
```

- Declaring a name and initialising it later, the compiler prevents you from using it before its initialised

```rust
let x;
foobar(x); // error: borrow of possibly-uninitialized variable: `x`
x = 42;
```

- This is fine

```rust
let x;
x = 42;
foobar(x); // type of x inferred here
```

- The underscore `_` is a **special name** , or *lack of name* that means to throw something away

```rust
// this does *nothing* because 42 is a constant
let _ = 42;

// this calls `get_thing` but throws away its result
let _ = get_thing();
```

- Names that start with an underscore are regular names, its just the compiler will not warn them being unused

```rust
let _x = 42; // we may use `_x` eventually, but our code is a work-in-progress
// and we just wanted to get rid of a compiler warning for now.
```

- Separate bindings with the same name can be introduced, you can **shadow** a variable binding

```cpp
let x = 13;
let x = x + 3;
// using `x` after that line only refers to the second `x`,
// the first `x` no longer exists.
```

- Rust has **tuples**, which are like **fixed length collections of values** of different types

```rust
let pair = ('a', 17);
pair.0; // this is 'a'
pair.1; // this is 17
```

- If we really wanted to annotate the type of `pair`, we write

```rust
let pair: (char, i32) = ('a', 17);
```

- Tuples can be **destructured** when doing an assignment, which means they are broken down to the individual fields

```rust
let (some_char, some_int) = ('a', 17);
// now, `some_char` is 'a', and `some_int` is 17
```

- This is useful when a function returns a tuple

```rust
let (left, right) = splice.split_at(middle);
```

- When destructuring a tuple, `_` can be used to throw away part of it.

```rust
let (_, right) = splice.split_at(middle);
```

- The semi colon marks the end of a statement

```rust
let x = 3;
let y = 5;
let z = y + x;
```

- Therefore we can do this

```rust
let x = vec![1,2,3,4,5,6,7,8]
	.iter()
	.map(|x| x + 3)
	.fold(0, |x,y| x + y);
```

- `fn` declares a function, here is a void function

```cpp
fn greet() {
    println!("Hi there!");
}
```

- Here is a function that returns a 32-bit signed integer, arrow indicating its return type.

```rust
fn fair_dice_roll() -> i32 {
    4
}
```

- A pair of brackets declares a block, which has its own scope

```rust
fn main(){
    let x = "out";
   	{
        let x = "in";
        println!("{}", x);
    }
    println!("{}", x);
}
```

- Blocks are also expressions, which mean they evaluate to some value

```rust
let x = 42; // this
let x = {42}; // equivalent to this
```

- Inside a block, there can be multiple statements ==notable==

```rust
let x = {
    let y = 1; // first statement
    let z = 2; // second statement
    y + x; // this is the "tail", which is what the whole block evaluates to
}
```

- This is why **omitting the semicolon** at the end of a function is the same as returning
- These are equivalent:

```rust
fn fair_dice_roll() -> i32 {
    return 4;
}

fn fair_dice_roll() -> i32 {
    4
}
```

- `if` conditionals are also expressions

```rust
fn fair_dice_roll() -> i32 {
    if feeling_lucky {
        6
    } else {
        4
    }
}
```

- A `match` is also an expression

```rust
fn fair_dice_roll() -> i32 {
    match feeling_lucky {
        true => 6,
        false => 4,
    }
}
```

- Dots are typically used to access fields of a value

```rust
let a = (10, 20);
a.0; // this is 10

let amos = get_some_struct();
amos.nickname; // this is fasterthanlime"
```

- Or call a method on a value

```rust
let nick = "fasterthanlime";
nick.len(); // this is 14.
```

- The double colon, `::` is similar but operates on namespaces
- In this example, `std` is a *crate* (a **library**), `cmp` is a *module* (**source file**) and `min` is a *function*. ==notable==

```rust
let least = std::cmp::min(3, 8); // this is 3
```

- `use` directives can be used to **bring in scope** names from other namespaces

```rust
use std::cmp::min;
let least = min(7,1); // this is 1
```

- Within `use` directives, curly brackets have another meaning, they are **globs**.
- If we want to import both `min` and `max`, we can do any of these:

```rust
// this works:
use std::cmp::min;
use std::cmp::max;

// this also works:
use std::cmp::{min, max};

// this also works!
use std::{cmp::min, cmp::max};
```

- A wildcard `*` lets you  import every symbol from a namespace:

```rust
use std::cmp::* // bring min and max in scope, with many other things also
```

- Types are namespaces too, and methods can be called as regular functions

```rust
let x = "amos".len(); // this is 4
let x = str::len("amos"); // this is also 4
```

- `str` is a primitive type, but many non primitive types are also in scope by default.

```rust
// 'Vec' is a regular struct, not a primitive type
let v = Vec::new();

// this is exactly the same code, but with the full path to 'Vec'
let v = std::vec::Vec::new();
```

- This works because rust inserts this as the beginning of every module ==notable==

```rust
use std::prelude::v1::*;
```

- Which in turns re-exports a lot of symbols such as `Vec`, `String`, `Option` and `Result`
- Structs are declared with the `struct` keyword.

```rust
struct Vec2 {
    x: f64, // double precision (64 bit float)
    y: f64
}
```

- They can be initialised using *struct literals* ==notable== 

```rust
let v1 = Vec2 { x: 1.0, y: 3.0 };
let v2 = Vec2 { y: 2.0, x: 4.0 };
// the order does not matter, only the names do
```

- There is a shortcut for initialising the rest of the fields from another struct. ==notable==

```rust
let v3 = Vec2 {
    x: 14.0,
    ..v2
}
```

- This is the **struct update syntax**, and can only happen in the **last position**, and cannot be followed by a comma.
- The rest of the fields can mean **all the fields**

```rust
let v4 = Vec2 { ..v3 };
```

- Structs, like tuples, can be destructured. ==notable==
- This is a valid `let` pattern:

```rust
let (left, right) = slice.split_at(middle);
```

- So is this.

```rust
let v = Vec2 {x: 3.0, y:6.0};
let Vec2 {x,y} = v;
// `x` is now 3.0, `y` is now `6.0`
```

- And this

```rust
let Vec2 {x, ..} = v;
// this throws away v.y
```

- `let` patterns can be used as conditions in `if` ==notable== 

```rust
struct Number {
    odd: bool,
    value: i32,
}

fn main() {
    let one = Number { odd: true, value: 1 };
    let two = Number { odd: false, value: 2 };
    print_number(one);
    print_number(two);
}

fn print_number(n: Number) {
    if let Number { odd: true, value } = n {
        println!("Odd number: {}", value);
    } else if let Number { odd: false, value } = n {
        println!("Even number: {}", value);
    }
}

// this prints:
// Odd number: 1
// Even number: 2
```

- `match` arms are also patterns,  just like `if let`  ==notable== 

```rust
fn print_number(n: Number) {
    match n {
        Number {odd: true, value} => println!("Odd number: {}\n", value),
        Number {odd: false, value} => println!(2)
    }
}
```

- A `match` has to be exhaustive: at least one arm needs to match.

```rust
fn print_number(n: Number) {
    match n {
        Number { value: 1, .. } => println!("One"),
        Number { value: 2, .. } => println!("Two"),
        Number { value, .. } => println!("{}", value),
        // if that last arm didn't exist, we would get a compile-time error
    }
}
```

- If that is hard  `_` can be used as a **catch all pattern** ==notable==

```rust
fn print_number(n: Number) {
    match n.value {
        1 => println!("One"),
        2 => println!("Two"),
        _ => println!("{}", n.value),
    }
}
```

- You can declare methods on your own types

```rust
struct Number {
    odd: bool,
    value: i32,
}

impl Number {
    fn is_strictly_positive(self) -> bool {
        self.value > 0
    }
}
```

- Use them as normal

```rust
fn main() {
    let minus_two = Number {
        odd: false,
        value: -2,
    };
    println!("positive? {}", minus_two.is_strictly_positive());
    // this prints "positive? false"
}
```

- Variable bindings are **immutable by default**, which means the interior cannot be mutated ==notable== 

```rust
fn main(){
    let n = Number {
        odd: true,
        value: 17
    }
    n.odd = false; // error: cannot assign to `n.odd`, as `n` is not declared to be mutable
}
```

- They can also not be assigned to:

```rust
fn main() {
    let n = Number {
        odd: true,
        value: 17,
    };
    n = Number {
        odd: false,
        value: 22,
    }; // error: cannot assign twice to immutable variable `n`
}
```

- `mut` makes a variable binding mutable

```rust
Rust code

fn main() {
    let mut n = Number {
        odd: true,
        value: 17,
    }
    n.value = 19; // all good
}
```

- **Traits** are something multiple types can have in common ==notable==

```rust
trait signed {
    fn is_strictly_negative(self) -> bool;
}
```

- You can implement:
  - One of your traits on anyone's type
  - Anyone's trait on one of your types
  - But not a foreign trait on a foreign type
- These are the **orphan rules**
- Here is an implementation of our trait on our type

```rust
impl Signed for Number {
    fn is_strictly_negative(self) -> bool {
        self.value < 0
    }
}

fn main(){
    let n = Number {odd: false, value: -44};
    println!("{}", n.is_strictly_negative()); // prints `true`
}
```

- A **foreign trait** on our type ==notable==

```rust

// The `Neg` trait is used to overload `-`, the unary minus operator
impl std::ops::Neg for Number {
    type Output = Number;
    
    fn neg(self) -> Number {
        Number {
            value: -self.value,
            odd: self.odd
        }
    }
}

fn main(){
    let n = Number {odd: true, value: 987};
    let m = -n; // this is only possible because we implemented `Neg`
    println!({}, m.value); // "-987"
}
```

- An `impl` block is always for a type, so inside that block, `Self` means that type

```rust
impl std::ops::Neg for Number {
    type Output = Self; // Self is our `Number`
    
    fn neg(self) -> Self {
        Self {
            value: -self.value,
            odd: self.odd
        }
    }
}
```

- Some traits are **markers** - they do not say that a type implements some methods, they say certain things can be done with a type.
- `i32` implements the trait `Copy` so `i32` *is* `Copy` , therefore this works ==notable== 

```rust
fn main() {
    let a: i32 = 15;
    let b = a; // `a` is copied
    let c = a; // `a` is copied again
}
```

- And this also works

```rust
fn print_i32(x: i32) {
    println!("x = {}", x);
}

fn main() {
    let a: i32 = 15;
    print_i32(a); // `a` is copied
    print_i32(a); // `a` is copied again
}
```

- But the `Number` Struct is not `Copy`, so this does not work ==notable==

```rust
fn main() {
    let n = Number { odd: true, value: 51 };
    let m = n; // `n` is moved into `m`
    let o = n; // error: use of moved value: `n`
}
```

- And neither does this:

```rust
fn print_number(n: Number) {
    println!("{} number {}", if n.odd { "odd" } else { "even" }, n.value);
}

fn main() {
    let n = Number { odd: true, value: 51 };
    print_number(n); // `n` is moved
    print_number(n); // error: use of moved value: `n`
}
```

- But it works if `print_number` takes an immutable reference instead:

```rust
fn print_number(n: &Number) {
    println!("{} number {}", if n.odd { "odd" } else { "even" }, n.value);
}

fn main() {
    let n = Number { odd: true, value: 51 };
    print_number(&n); // `n` is borrowed for the time of the call
    print_number(&n); // `n` is borrowed again
}
```

- It also works if a function takes a *mutable reference* - but only if our variable binding is also `mut`

```rust
fn invert(n: &mut Number) {
    n.value = -n.value;
}

fn print_number(n: &Number){
    println!("{} number {}", if n.odd {"odd"} else {"even"}, n.value);
}

fn main(){
    // this time `n` is mutable
    let mut n = Number {odd: true, value: 51};
    print_number(&n);
    invert(&mut n); // n is borrwed mutably - everything is explicit
}
```

- Trait methods can also take `self` by reference or mutable references

```rust
impl std::clone::Clone for Number {
    fn clone(&self) -> Self {
        Self {..*self}
    }
}
```

- When invoking trait methods, the **receiver is borrowed implicitly** ==notable== 

```rust
fn main() {
    let n = Number { odd: true, value: 51 };
    let mut m = n.clone(); // n is the receiver and is borrowed implicitly
    m.value += 100;
    
    print_number(&n);
    print_number(&m);
}
```

- To highlight this: these are equivalent

```rust
let m = n.clone();

let m = std::clone::Clone::clone(&n);
```

- Marker traits like `Copy` have no methods

```rust
// note: `Copy` requires that `Clone` is implemented too
impl std::clone::Clone for Number {
    fn clone(&self) -> Self {
        Self { ..*self }
    }
}
```

- To highlight this: these are equivalent 

```rust
let m = n.clone();

let m = std::clone::Clone::clone(&n);
```

- Marker traits like `Copy` have no methods

```rust
// note: `Copy` requires that `Clone` is implemented too
impl std::clone::Clone for Number {
    fn clone(&self) -> Self {
        Self { ..*self }
    }
}

impl std::marker::Copy for Number {}
```

- Now `Clone` can still be used:

```rust
fn main() {
    let n = Number { odd: true, value: 51 };
    let m = n.clone();
    let o = n.clone();
}
```

- But `Number` values will no longer be moved

```rust
fn main(){
    let n = Number {odd: True, value: 51};
    let m = n; // `m` is a copy of `n`
    let o = n; // same. `n` if neither moved nor borrowed
}
```

- Some traits are so common, they can be implemented automatically by using the `derive` attribute.

```rust
#[derive(Clone, Copy)]
struct Number {
    odd: bool,
    value: i32,
}

// this expands to `impl Clone for Number` and `impl Copy for Number` blocks.
```

- Functions can be generic

```rust
#[derive(Clone, Copy)]
struct Number {
    odd: bool,
    value: i32,
}

// this expands to `impl Clone for Number` and `impl Copy for Number` blocks.
```

- Functions can be generic:

```rust
fn foobar<T>(arg: T){
    // do something `arg` 
}
```

- They can have multiple *type parameters*, which can then be used in the functions declaration and its body, instead of concrete types:

```rust
fn foobar<L, R>(left: L, right: R){
    // do something with `left` and `right`
}
```

- Type parameters usually have *constraints* so you can actually do something with them
- The simplest constraints are just trait names:

```rust
fn print<T: Display>(value: T){
    println!("value = {}", value);
}

fn print<T: Debug>(value: T){
    println!("value = {:?}", value);
}
```

- There's a longer syntax for type parameter constraints

```rust
fn print<T>(value: T)
where
    T: Display,
{
    println!("value = {}", value);
}
```

- Constraints can be more complicated: they require a type parameter to implement multiple traits:

```rust
use std::fmt::Debug;

fn compare<T>(left: T, right: T) 
where 
	T: Debug + PartialEq, 
{
    println!("{:?} {} {:?}", left, if left == right { "==" } else { "!=" }, right);
}

fn main() {
    compare("tea", "coffee");
    // prints: "tea" != "coffee"
}
```

- Generic functions can be thought of as namespaces, containing an infinite number of functions different concrete types
- Same as with crates, modules and types, generic functions can be **explored** using `::` ==notable== 

```rust
fn main() {
    use std::any::type_name;
    println!("{}", type_name::<i32>()); // prints "i32"
    println!("{}", type_name::<(f64, char)>()); // prints "(f64, char)"
}
```

- This is called **turbofish syntax** because `::<>` looks like a fish.
- Structs can also be generic:

```rust
struct Pair<T> {
    a: T,
    b: T,
}

fn print_type_name<T>(_val: &T) {
    println!("{}", std::any::type_name::<T>());
}

fn main() {
    let p1 = Pair { a: 3, b: 9 };
    let p2 = Pair { a: true, b: false };
    print_type_name(&p1); // prints "Pair<i32>"
    print_type_name(&p2); // prints "Pair<bool>"
}
```

- The standard library type `Vec` (`~` a heap allocated array), is generic

```rust
fn main() {
    let mut v1 = Vec::new();
    v1.push(1);
    let mut v2 = Vec::new();
    v2.push(false);
    print_type_name(&v1); // prints "Vec<i32>"
    print_type_name(&v2); // prints "Vec<bool>"
}
```

- Speaking of `Vec`, it comes with a macro that gives more or less **vec literals** ==notable== 

- All of `name!()`, `name![]` and `name!{}` invoke a macro, macros just expand to regular code ==notable==
- `println` is a macro.

```rust
fn main() {
    println!("{}", "Hello there!");
}
```

- This expands to something that has the same effect as

```rust
fn main() {
    use std::io::{self, Write};
    io::stdout().lock().write_all(b"Hello there!\n").unwrap();
}
```

- `panic` is also a macro. It violently stops execution with an error message, and the file name / line number of the error, if enabled

```rust
fn main() {
    panic!("This panics");
}
// output: thread 'main' panicked at 'This panics', src/main.rs:3:5
```

- Some methods also panic, for example the `Option` type can contain something, or it can contain nothing. If `.unwrap()` is called on it, and it contains nothing , it panics

```rust
fn main() {
    let o1: Option<i32> = Some(128);
    o1.unwrap(); // this is fine

    let o2: Option<i32> = None;
    o2.unwrap(); // this panics!
}

// output: thread 'main' panicked at 'called `Option::unwrap()` on a `None` value', src/libcore/option.rs:378:21
```

- `Option` is not a struct, its an `enum` with two variants ==notable== 

```rust
enum Option<T> {
    None,
    Some(T),
}

impl<T> Option<T> {
    fn unwrap(self) -> T {
        // enums variants can be used in patterns:
        match self {
            Self::Some(t) => t,
            Self::None => panic!(".unwrap() called on a None option"),
        }
    }
}

use self::Option::{None, Some};

fn main() {
    let o1: Option<i32> = Some(128);
    o1.unwrap(); // this is fine

    let o2: Option<i32> = None;
    o2.unwrap(); // this panics!
}

// output: thread 'main' panicked at '.unwrap() called on a None option', src/main.rs:11:27
```

- `Result` is also an enum, it can either contain something, or an error

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

- It also panics when unwrapped containing an error ==notable==
- Variable bindings have a lifetime

```rust
fn main() {
    // `x` doesn't exist yet
    {
        let x = 42; // `x` starts existing
        println!("x = {}", x);
        // `x` stops existing
    }
    // `x` no longer exists
}
```

- References have a lifetime also

```rust
fn main() {
    // `x` doesn't exist yet
    {
        let x = 42; // `x` starts existing
        let x_ref = &x; // `x_ref` starts existing - it borrows `x`
        println!("x_ref = {}", x_ref);
        // `x_ref` stops existing
        // `x` stops existing
    }
    // `x` no longer exists
}
```

- The lifetime of a reference cannot exceed the lifetime of the variable binding it borrows.

```rust
fn main() {
    let x_ref = {
        let x = 42;
        &x
    };
    println!("x_ref = {}", x_ref);
    // error: `x` does not live long enough
}
```

- A variable binding can be immutably borrowed multiple times

```rust
fn main() {
    let x = 42;
    let x_ref1 = &x;
    let x_ref2 = &x;
    let x_ref3 = &x;
    println!("{} {} {}", x_ref1, x_ref2, x_ref3);
}

```

- While borrowed, a variable binding may **not be mutated**. ==notable== 

```rust
fn main() {
    let mut x = 42;
    let x_ref = &x;
    x = 13;
    println!("x_ref = {}", x_ref);
    // error: cannot assign to `x` because it is borrowed
}
```

- While immutably borrowed, a variable cannot be **mutably borrowed**

```rust
fn main() {
    let mut x = 42;
    let x_ref1 = &x;
    let x_ref2 = &mut x;
    // error: cannot borrow `x` as mutable because it is also borrowed as immutable
    println!("x_ref1 = {}", x_ref1);
}
```

- References in function arguments also have lifetimes

```rust
fn print(x: &i32) {
    // `x` is borrowed (from the outside) for the
    // entire time this function is called.
}
```

- Functions with reference arguments can be called with borrows that have different lifetimes, so ==notable== 
  - All functions that take references are generic
  - Lifetimes are generic parameters
- Lifetimes names start with a single quote `'`.

```rust
// elided (non-named) lifetimes:
fn print(x: &i32) {}

// named lifetime
fn print<'a>(x: &'a i32){}
```

- This allows returning references whose lifetime depend on the lifetime of the arguments

```rust
struct Number {
    value: i32,
}

fn number_value<'a>(num: &'a Number) -> &'a i32 {
    &num.value
}

fn main() {
    let n = Number { value: 47 };
    let v = number_value(&n);
    // `v` borrows `n` (immutably), thus: `v` cannot outlive `n`.
    // While `v` exists, `n` cannot be mutably borrowed, mutated, moved, etc.
}
```

- When there is  a *single* input lifetime, it does not need to be named, and everything has the same lifetime, so the two functions below are equivalent

```rust
fn number_value<'a>(num: &'a Number) -> &'a i32 {
    &num.value
}

fn number_value(num: &Number) -> &i32 {
    &num.value
}
```

