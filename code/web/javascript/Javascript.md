# JavaScript

primitive types - null, boolean, string integer, undefined, symbol , BigInt

use clear() to wipe values from the chrome console.

Java consoles understands REPL where it just reads and outputs the lines.

recall previously run code with the console.

##### Numbers - console

- There are negative numbers, decimal numbers, positive numbers, integers.
- There are **NaN** numbers, this represents a value that is not a number such as 0/0
  - Also 1+ NaN

```javascript
// Javscript comment

50 + 5 
55
55%5
0

2**4 // raise to power 2^4.
```

```js
0/0 // outputs NaN

NaN * 1 = NaN
```

```javascript
typeof <data> // returns type a variable is 
    
typeof 4 // returns "number"
```

```javascript
// variables 
// let variableName = data;

let year = 1985;

console.log(year) // 1985.

let year = 1985 +2 // stores 1987 

```

```javascript
let score = 0;
>0

score = 5;

score += 5;
score *= 5
score -= 1;

score-- // decrements post, therefore use the value score then decrement. 

```

```javascript
const hens = 4 // contstant

hens = 20; // error
```

```
var runDistance = 26.2;

runDistance
```

difference between let and var -= https://stackoverflow.com/questions/762011/whats-the-difference-between-using-let-and-var

```javascript
let gameOver = false;

let gameOver = true;
```

```javascript
let foo = false;
let foo = 2; // replace false with 2 , as there is no datatype defined on it
```

## Strings

```javascript
let userName = "charles";

username
> charles
```

```javascript
let name = "charles";

name[0] // access 'c'

// returns undefined if out of bounds

name.length > returns number of characters in string


"xd" + " balls" > "xd balls" // concatenation

// strings cannot be updated or changed , it makes a new string always in memory/ overwrite the string. 

1 + "hi" > "1hi" //same result as java, it converts the 1 to a string in order to make it work
```

String methods

```javascript
let name = "simon"

simon.length > 5

//thing.method

simon.toUpperCase() > all to uppercase
// this does not destroy the name variable

let trimThis = "    whitespace    ";

trimThis.trim() > "whiteslace" // removes on left and right and not inbetween each word

let trimThis2 = " This wont fully trim ";

trimThis.trim() > "This wont fully trim"

let foo = "hello you cunt";

let.toUpperCase.trim() > //new copy with trimming and upper case
    
 
```

```javascript
let tvShow = 'catdog ';

// same as pythons find function . Only does the first occurence. 
tvShow.indexOf('cat') // returns 0 as this is the start of the instance
tvShow.indexOf('z') // returns -1 for not found
tvShow.indexOf('dog') // returns 3

// can do index of empty space. 

let str = "testword"

str.slice(0,5) // start from index 0 up to but not including index 5 > testw 

str.slice(0,1) // t

str.slice(5)// removes first 5 characters in the streing

// This can work on arrays and containers too. 
```

```javascript
let test = "replace this phrasej"

test.replace("replace", "fuck you") > "fuck you this phrase"

test.repeat(10) //creates a string with the phrase concatentaed 10 times
```

String template literals

```javascript
let product = "Artichoke";

let price = 3.99;

let qty = 5;

"You bought " + qty + " " + product + ". Total is: " + price * qty" > you bought 5 artichoke. total is 11.25

//Use template literals to embed data into the expressions of the strings. 

// note you must use the backtick variable to specify this
`i hate myself so much ${3+4}`; // i hate myself so much 7

let fuck = "bitch";

`yep ${fuck}` > "yes bitch"
```

##### Null / defined

- Null
  - "Intentional absence of any value"
  - Must be assigned
  - The user has chosen for a variable to be empty on purpose. 
- Undefined
  - Variables that do not have an assigned value are undefined. 
  - Out of bounds errors is an example of this

```

```

Math object

- rounding
- absolute value
- PI
- pow
- floor

```javascript
Math.PI // 3.14

Math.floor(23.999) //chops off the decimal > 23

Math.ceil(34.1) // rounds to next highest integer  > 35 

// Random Numbers // 

Math.random() // default random between 0 and 1 

// example random numbers between 1 and 10 //

Math.floor(Math.random() * 10) + 1; // replace 1 and 10 to obtain any range , 1 is used as it does not include 10 initially
// 10 indicates how many numbers are being generated in the random sequence

Math.floor(Math.random() * 3) + 20; // obtain values ranging from 20-22

Math.pow(2,3) // 2^3
```

### Decision making

- comparison statements return true or false.
- **==** - checks for equality of value, but not equality of type
  - Coerces both values to the same type and then compares them
  - May lead to unexpected results. 
- **===** - does care about the type of a value therefore for example the number 1 differs from the character for 1. 

```javascript
//Double equals

// Checks for equality of value, but not equality of a type

1 == '1' // true as it switches the type to make it equal. 

0 == ''; // true

null = undefined // true

0 == false // true

// Triple equals

1 === '1' // false

// Same rules applies for not equals in strict and non strict versions. 
```

#### Console alert and prompt

```javascript
console.log(arg) // prints arg, required when working in a file. 

console.warn(arg) // prints out arg with yellow warning sign next to it.

console.error(arg) // prints out error with cross sign next to it

alert(arg) //display a pop up to user with arg

let number = prompt("please enter a number"); // allow the user to enter a number that prompt captures and stores in number, from within the console, often just use forms for this however. 

parseInt(nonIntValue) // extracts the number from the argument passed in for example 101xxx returns 101 when parsed by int




```



https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment - destructuring

## DOM

```
document.all[10] // select an object in the document 

document.all[10].innerText = 'newItem' // select innerText item, and change the value if a text tag to newItem.

document.getElementById('banner') // returns image html object which can modify the properties of them.
```

# Asynchronous JavaScript

### Call Stack

- The mechanism the JS interpreter uses to **keep track** of its place in a **script** that calls multiple functions

- It is how **js** *knows* what function is currently **being executed** and what functions are called from within that function etc.

---

- When a script **calls a function** $\to$ the **interpreter** adds it to the **call stack** and then starts carrying out the function.
- Any **functions** that are **called** by **that function** $\to$ are **added** to the call stack but **further up** , and then **run** where their **calls** are **reached**.
- When the **current function** is **finished** $\to$ the interpreter takes it off the stack and **resumes execution** where it left off in the **last code listing**.

http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D - callstack visualiser

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201227073751957.png)

debugging be like ^^^^

### Web APIs and single threaded

- It can only run a singular stream of execution at once
- It can not execute in parallel.

---

- As JavaScript can only run at most one piece of code at a time it must keep executing other code whilst waiting on some resource to be loaded in order to stop the program from freezing.

### Ajax

- Async
- Javasript
- and
- XML

---

- Used to load requests from servers to load new data when the page has to obtain more information.
- Sends request for data to be fetched back in JSON.

### APIs

- Application programming interface
- Software communicating with each other.
- Applications have endpoints that you can send API requests too in order to fetch data from.
- May cost money for servers to respond to requests.
- facebook / twilio. 

### XML / JSON

- JSON - javascript object notation.
- every key must be in double quotes.

https://www.json.org/json-en.html

https://jsonformatter.curiousconcept.com/

https://www.cryptonator.com/api

### Postman

- headers
- status codes are not displayed.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201228210102737.png)

- postman HTTP various types of requests.
- GET -> RETRIEVE
- POST -> SEND

---

- Body -> is the raw content of a http response.
- http status code -> determine if the connection was ok such as 200 for OK and 404 not found.
- POST -> 405 status if not allowed to post the dedicated resource.

https://developer.mozilla.org/en-US/docs/Web/HTTP/Status

##### Headers

- key value pairs , metadata.
- include stuff like date, last modified time, content tpe

https://developer.mozilla.org/en-US/docs/Web/API/Headers

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201228211213629.png)

- not the ?q=css, is what we fill in.
- Use this in order to fetch values relating to a certain subject such as on tv maze api using the query for girls when finding shows with girls in the name.
- Show lookup use the lookup tab then provide the correct endpoint of a show to view.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201228211539316.png)

- use & to string query search onto each other.

- Essentially a key value pair that is used to request data.

https://icanhazdadjoke.com/api

- Add the accept header to this request in order to fetch a dad joke.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201228212118548.png)

- changed the header and value to fetch the required JSON section

### Making XHR

- XMLHTTPRequest

---

- The OG way of sending requests via JS
- Does not support promises promises, lots of call-backs
- what is with this weird capitalization.
- Clunky syntax, hard to remember

### Fetch API

- Newer method making requests via JS
- Supports promises
- Not supported in internet explorer.

### Axios

- library for making HTTP requests

- Turns the two promise steps into one section.

# Object Oriented Programming

### Object prototypes

- Prototype object acts as a **template object** that it **inherits methods** and **properties from**.
- Such as an array , the methods built into it are built into its **proto** method

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20201230044834975.png)

- means instances of arrays dont all carry their own seperate methods and rather just inherit from the array class to use its methods instead.

- They are like a **template object**.

---

- String prototype methods contain the string object wrapper methods.
- Can define our own prototype : String.prototype.balls = () => alert("i just dont like anything anymore.")
- This means that we can now call this balls method on every new string defined.

#### New operator and constructor functions

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/new - new operator. 



