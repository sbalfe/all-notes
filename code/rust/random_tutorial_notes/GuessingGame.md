# Guessing Game

```rust

/* obtain user input/output from standard library*/
use std::io;

/*
	Prelude - standard library that does not need to `use` at the top.
*/
fn main (){



    println!("Guess the number");
    println!("Please input the guess");

    /* Variables are immutable by default 

        `=` means to bind `guess` to `String::new()` 

        `String` > type provided by the standard library.
                 > growable UTF-8 encoded bit of text.

        `::` > associated with String, meaning its implemented on String type.

        `::new()` > creates a new empty string
                  > common on many types for empty construction.

    */
    let mut guess = String::new();

    /* 
        std::in > handle user input 

        read_line > take whater the user types to std input and append into a string
                  > pass our empty string as the input to place in.

        &mut guess > must be mutable guess passed by reference so it can be updated.
                   > important : &mut is a mutable reference opposed to &guess which is an immutable reference.

        read_line > returns io::Result , special return specifically for io::
                  > Result types are enumrations > often used with match
                  > Result encodes error handling information
                    > OK variant = operation successful
                    > Err variant = operation failed, contains information why it failed.

        expect > defined on io::Result as a methods
               > if io::Result is an Err variant > program crashes and displays message that crashed program.
               > if read_line returns Err variant > result of error from underlying OS
               > if io::Result is Ok > expect take return value that Ok is holding and return the value to use.
               > value for our case is the number of bytes.
               > A warning is generated without the expect statement.

    */s


    io::stdin()
        .read_line(&mut guess)
        .expect("failed to read line");

    println!("You guessed: {}", guess);


}
```

