# Summary of rust moving, borrowing and mutability

> https://users.rust-lang.org/t/rust-mutability-moving-and-borrowing-the-straight-dope/22166 

- Move - transfer ownership
- Borrow - pass a reference
- Copy - copy to a function, implement the `Copy` Trait, avoids the Move into a function
- Mutability - keyword `mut` to determine whether said variable can be changed, may be in the form of a `&mut` to describe  a mutable reference or `let mut` to initialise a mutable variable 