# Using Strings

## String class as a container

- Strings are based on `basic_string` **template class**
  - This is a **container** therefore has **uses iterator access** and **methods** to *obtain information*
    - Alongside having **template parameters** that contain **information** about the **character type** it holds
- There are various **typedef** for certain **character types**

---

```c++
   typedef basic_string<char,
       char_traits<char>, allocator<char> > string; 
    typedef basic_string<wchar_t,
       char_traits<wchar_t>, allocator<wchar_t> > wstring; 
    typedef basic_string<char16_t,
       char_traits<char16_t>, allocator<char16_t> > u16string; 
    typedef basic_string<char32_t,
       char_traits<char32_t>, allocator<char32_t> > u32string;
```

- String > based on ***char***
- wString > based ***wchar_t***
- 16string  / u32 string > 16 bit and 32 bit characters respectively.
- Concentrate on the **string for now**

----

- Comparing / copying / accessing characters in a string requires **different code**
  - For different **sized characters**
    - While **traits** template what **parameter** provides the **implementations**
- For *string* $\to$ this is **char_traits** class
  - When this class **copies characters** $\to$ it **delegate** this **action** to **char_traits** class and its **copy method.**
    - The **traits classes** used by **stream classes**
- They also **define** an **EOF** that is **appropriate** to the **file stream**.

---

- String is just an **array** of **zero or more characters** that *allocates memory* when required and **deallocates** on **removal** of the **string object**
  - Some respects $\to$ similar to `vector<char>` 
    - As a container $\to$ the **string** class gives **iterator access** via `begin` \ `end` methods.

```c++
    string s = "hellon"; 
    copy(s.begin(), s.end(), ostream_iterator<char>(cout));
```

- Here
  - The **begin and end** methods are called to obtain **iterators** on the **string object** 
    - Passed to the **copy** function from `<algorithm>` to **copy each character** to the **console** via the `ostream_iterator` temp object
  - In respect $\to$ the **string object** is *similar* to a **vector**
    - Use the **previously defined** $s$ object.

```c++
vector<char> v(s.begin(), s.end());
copy(v.begin(), v.end(), ostream_iterator<char>(cout));
```

- Fills the **vector** object using the **range of characters** given by `begin` and `end` methods on the **string object**
  - Prints **those characters** to the **console**  via **copy** function in the same as **previously seen**

## Getting information on a string

- The `max_size` method gives you the **maximum size** of the *string* of a specified **character type** #
- 64 bit windows > with **2GB memory** 
- max_size for a **string** object will **return 4 billion characters** , *wstring* object method returns **2 billion characters**

---

- `length` method returns **same value** as the **size method**
  - This is **how many items (characters)** are within the string
- `capacity` method indicates **how much memory** is already allocated for the string in terms of **number of chararacters** 

---

- Compare strings via calling `compare` method
  - Returns an **int** and **not** a **bool**
    - Note that an **int** can be *converted* silently **to a bool** 
- 0 = the same 
- negative if they are not the same, if the **parameter** is *less* than **operand string** is *greater* than the **operand string**
  - Vice versa for positive.
- greater / less test the **ordering** of strings **alphabetically**
  - Global operators are **define to compare string objects**

---

- `string` object can be used like a **C string** via the `c_str` method
  - Pointer return is **const**
    - NOTICE THAT IT **MAY BE INVALIDATED** if the *string* object is changed $\to$ therefore **do not store** this pointer.

---

- Do **not use** `&str[0]` to obtain a **C string** pointer for the **C++** string *str* as the **internal buffer** used via **string classes**
  - is not **certainly** going to be **null terminated**.
    - The `c_str` method is provided to **return a pointer** that can be **used** as a **string** 
      - Therefore is **null terminated**.

---

- To **copy data** from the **C++** string to a **C buffer** and call the **copy method**
  - Pass the **destination pointer** and the **number of characters** to *copy as parameters* (optionally an **offset**)
    - method attempts to **copy ** at most $\to$ the **specified number of characters**.
      - Should take the **steps** to **ensure this** 
- Has **no terminal character** $\to$ method assumes that the **destination buffer** is *large enough* to **hold** the **copied characters**
  - You should **take steps** to *ensure this* 
- To pass the **size of the buffer** so that the method **performs** the *check for you*
  - Call `_Copy_s` method instead

## Altering strings.

- String classes have **standard container** access methods > [] / .at
- Repalce the **entire strig** using `assign` method
- or **swap** contents of two string objects with `swap` method 
- Can **insert characters** in *specified places* with `insert` method
- **Remove** specific characters with `erase` method
- Remove **all characters** with `clear` method.
- Class allows you to **push characters** to the **end of the string** / remove the last character
  - With the **push_back** and **pop_back**

```c++
    string str = "hello"; 
    cout << str << "n"; // hello 
    str.push_back('!'); 
    cout << str << "n"; // hello! 
    str.erase(0, 1); 
    cout << str << "n"; // ello!
```

- Can **add one** or *more characters* to **end** via *append* method or `+=` operator

```c++
    string str = "hello"; 
    cout << str << "n";  // hello 
    str.append(4, '!'); 
    cout << str << "n";  // hello!!!! 
    str += " there"; 
    cout << str << "n";  // hello!!!! there
```

- `<string>` library defines a **global** `+` operator that **concatenate** two strings in a **third string**
  - To **change characters** in a *string* $\to$ acces via an **index** with `[]` operator
    - using the **reference** to **overwrite** the **character**
- Also use the **replace** method to **replace** *one / more* characters at **specified position** with *characters* from a **C string** / **C++ string**
- Or some other contained accessed via **iterators**

```c++
    string str = "hello"; 
    cout << str << "n";    // hello 
    str.replace(1, 1, "a"); 
    cout << str << "n";    // hallo
```

- Can **extract** part of a **string** as a **new string**
  - `substr` method takes an **offset** an **optional count**
- If the **count** of **characters** is *ommitted* $\to$ the **substring** will be from **specified position** 
  - Until the **end of the string** 
    - Therefore you can **copy** a **left hand** part of a **string** by *passing an offset* of **0** 
    - Alongside a **count** that is **less than the size of the string**
    - Can **copy** a **right hand part** of the *string* by *passing the index* of **first character.**

```c++
    string str = "one two three"; 
    string str1 = str.substr(0, 3);  
    cout << str1 << "n";          // one 
    string str2 = str.substr(8); 
    cout << str2 << "n";          // three
```

- Second example $\to$ copying starts at the **eight character** and **continues** to the **end**

## Searching strings

- `find` with C/C++ string , can provide **initial search position** 
  - Returns the **position** / rather than **iterator** to where the **search text** was **located**
  - Or value of **npos** if the **text** can *not be found*.
- The **offset parameter** along with a **successful return value** from the *find* method
  - Enables you **to parse a string repeatedly** to *find specific terms*
    - The `find` method **searches** for the **specified text** in the **forward direction**
      - Also an `rfind` method that **performs** the *search* in **reverse direction**.
- `rfind` is not the complete opposite of the **find method**
  -  The **find method** moves the **search point** forward in the **string**
    - At *each point* $\to$ *compares* the **search string** with the *characters* from the **search point forwards** (the first search search text character, then the second, so on)
  - `rfind` is **not** given an **offset**
    - The **first comparison** made at **an offset** from the **end** of the **string** $\to$ the *size* of the **search text**
- Comparison made by **comparing** the *first character* in the search text with the character at the **search point** in the **searched string**
  - If succeeds $\to$ the **second character** in the **search** is *compared* with the **character** after the **search point**
- Therefore the **comparisons** are made in a **direct opposite** direction to the **movement** of the **search point**

---

- This becomes vital to **parse a string** using the *return value* from the **find method** as an *offset*
  - After each search $\to$ should **move the search** *offset forwards* 
    - For `rfind` you should **move it backwards**
- Example $\to$ to **search all poistions** of *the* in the **following string**
  - Just call:

```c++
	string str = "012the678the234the890"; 
    string::size_type pos = 0; 
    while(true) 
    { 
        pos++; 
        pos = str.find("the",pos); 
        if (pos == string::npos) break; 
        cout << pos << " " << str.substr(pos) << "n"; 
    } 
    // 3 the678the234the890 
    // 9 the234the890 
    // 15 the890
```

- This will **find** the *search text* at character position of **3 / 9 / 15**
  - To *search* the **string backwards** $\to$ could call:

```c++
string str = "012the678the234the890"; 
    string::size_type pos = string::npos; 
    while(true) 
    { 
        pos--;   
        pos = str.rfind("the",pos); 
        if (pos == string::npos) break; 
        cout << pos << " " << str.substr(pos) << "n"; 
    } 
    // 15 the890 
    // 9 the234the890 
    // 3 the678the234the890
```

- The **highlighted** code shows the **changes** that *should be made*
  - Indicating that you need to **search** from the **end** and use the `rfind` method
    - When you have a **successful result** $\to$ must *decrement* the position before **next search**
- Like `find` method `rfind` returns **npos** if it *cannot* find the **search text.**

- There are **four methods** to search for *one* of **several individual characters**
  - Example:

```c++
    string str = "012the678the234the890"; 
    string::size_type pos = str.find_first_of("eh"); 
    if (pos != string::npos) 
    { 
        cout << "found " << str[pos] << " at position "; 
        cout << pos << " " << str.substr(pos) << "n"; 
    } 
    // found h at position 4 he678the234the890
```

- The **search string** `eh` and `find_first_of` return when it finds either **e / h**
- Can provide **offset parameter** to start the search 
  - Therefore use the **return value** to **parse** the **entire string**
- `find_last_of` is similar
  - Searches the **string** in **reverse direction** for one of the characters in the **search text.**

- There are **two search methods** that will look for a character other than the characters provided in search_text
  - `find_first_not_of` and `find_last_not_of` 

```c++
    string str = "012the678the234the890"; 
    string::size_type pos = str.find_first_not_of("0123456789"); 
    cout << "found " << str[pos] << " at position "; 
    cout << pos << " " << str.substr(pos) << "n"; 
    // found t at position 3 the678the234the890
```

- Code **looks for a character** other than a **digit**
  - Therefore find **t** at **position** 3 $\to$ the **4th character**
- There is **no library** function to **trim whitespace** from a *string*.
  - Can **trim spaces** on the **left** and **right** by using the **find functions** to locate **non whitespace**
    - Then use the correct **index** for `substr` method

```c++
    string str = "  hello  "; 
    cout << "|" << str << "|n";  // |  hello  | 
    string str1 = str.substr(str.find_first_not_of(" trn")); 
    cout << "|" << str1 << "|n"; // |hello  | 
    string str2 = str.substr(0, str.find_last_not_of(" trn") + 1); 
    cout << "|" << str2 << "|n"; // |  hello|
```

- One left trims / one right trims
- First forward searches for the **first character** that is **not whitespace**
  - Use as **start of index** of the **substring** (*no count* is provided as all of the **remaining string** is **copied**)
- For the **second case** $\to$ the *string* is **reverse searched** for a *character* that **is not whitespace**
  - The location returned is the **last character** of `hello` 
    - We require the **substring** from the **first character** $\to$ we increment the **index** to obtain the **count** of **characters** to **copy**.

http://www.cplusplus.com/reference/string/string/find_last_not_of/

## Internationalization

The <locale> header contains the classes for localizing how time, dates, and currency are formatted, and also to provide localized rules for string comparisons and ordering.

The C Runtime Library also has global functions to carry out localization. However, it is important in the following discussion that we distinguish between C functions and the C locale. The C locale is the default locale, including the rules for localization, used in C and C++ programs and it can be replaced with a locale for a country or culture. The C Runtime Library provides functions to change the locale, as does the C++ Standard Library.

Since the C++ Standard Library provides classes for localization, this means that you can create more than one object representing a locale. A locale object can be created in a function and can only be used there, or it can be applied globally to a thread and used only by code running on that thread. This is in contrast to the C localization functions, where changing the locale is global, so all code (and all threads of execution) will be affected.

Instances of the locale class are either created through the class constructor or through static members of the class. The C++ stream classes will use a locale (as explained later), and if you want to change the locale you call the imbue method on the stream object. In some cases, you will want to access one of these rules directly, and you have access to them through the locale object.

## Facets

- Internationalization rules are known as **facets**.
-  A locale object is a **container of facets**, and you can **test if the locale** has a specific facet using the has_facet function; if it does, you can get a const reference to the facet by calling the use_facet function. 
- There are six types of facets summarized by seven categories of class in the following table. A facet class is a subclass of the locale::facet nested class.

| **Facet type** | **Description**                                              |
| -------------- | ------------------------------------------------------------ |
| codecvt, ctype | Converts between one encoding scheme to another and is used to classify characters and convert them to upper or lowercase |
| collate        | Controls the ordering and grouping of characters in a string, including comparing and hashing of strings |
| messages       | Retrieves localized messages from a catalog                  |
| money          | Converts numbers representing currency to and from strings   |
| num            | Converts numbers to and from strings                         |
| time           | Converts times and dates in numeric form to and from strings |

- The facet classes are used to convert the **data** to strings and so they all have a **template** parameter for the **character** **type** used. 
- The money, num, and time facets are represented by three classes each. 
- A class with the '**_get** **suffix**' that handles **parsing strings**, while a class with the **_put suffix** handles **formatting as strings**. 
- For the money and num facets there is a class with the **punct suffix** that contains the rules and symbols for punctuation.

---

- Since the **_get facets** are used to **convert sequences of characters into numeric types**, the classes have a **template parameter** that you can use to indicate the **input iterator type** that the **get methods** will use to **represent a range of characters**.
-  Similarly, the **_put facet** classes have a **template parameter** that you can use to **provide the output iterator type** the put methods will write the **converted string to**. 
- There are **default types** provided for **both iterators types**.

---

- The messages facet is used for compatibility with POSIX code.
-  The class is intended to allow you to **provide localized strings** for your application.
-  The idea is that the **strings in your user interface** are indexed and at **runtime** you access the localized string using the index through the messages facet.
-  However, Windows applications typically use message resource files compiled using the **Message Compiler**.
-  It is perhaps for this reason that the messages facet provided as part of the Standard Library does not do anything, but the infrastructure is there, and you can derive your own messages facet class.

---

- The **has_facet** and **use_facet** functions are templated for the specific type of facet that you want.
-  All facet classes are subclasses of the locale::facet class, but through this template parameter the compiler will instantiate a function that returns the specific type you request. 
- So, for example, if you want to format time and date strings for the French locale, you can call this code:

```c++
    locale loc("french"); 
    const time_put<char>& fac = use_facet<time_put<char>>(loc);
```

- Here, the french string identifies the **locale**, and this is the language string used by the C Runtime Library setlocale function. 
- The second line obtains the facet for converting numeric times into strings, and hence the function template parameter is time_put<char>.
-  This class has a method called put that you can call to perform the conversion:

```c++
    time_t t = time(nullptr); 
    tm *td = gmtime(&t); 
    ostreambuf_iterator<char> it(cout); 
    fac.put(it, cout, ' ', td, 'x', '#'); 
    cout << "n";
```

- The time function (via <ctime>) returns an **integer** with the **current** **time** and **date**, and this is **converted to a tm** structure using the **gmtime** function. 
- The **tm structure** contains **individual members** for the year, month, day, hours, minutes, and seconds. 
- The **gmtime function returns** the address to a **structure** that is **statically allocated** in the function, so you **do not have to delete the memory** it occupies.

---

- The facet will format the data in the tm structure as a string through the output iterator passed as the first parameter. 
- In this case, the output stream iterator is constructed from the cout object and so the facet will write the format stream to the console (the second parameter is not used, but because it is a reference you have to pass something, so the cout object is used there too). 
- The third parameter is the separator character (again, this is not used). The fifth and (optional) sixth parameters indicate the formatting that you require.
-  These are the same formatting characters as used in the C Runtime Library function strftime, as two single characters rather than the format string used by the C function. In this example, x is used to get the date and # is used as a modifier to get the long version of the string.+

```c++
    samedi 28 janvier 2017 //code from above
```

- Notice that the words are **not capitalized** and there is no punctuation, also notice the order: weekday name, day number, month, then year.
- If the locale object constructor parameter is changed to german then the output will be:

```c++
    Samstag, 28. January 2017 // german
```

- Time is used as the example here because there is no stream, an insertion operator is used for the tm structure, and it is an unusual case.  
- For other types, there are insertion operators that put them into a stream, and so the stream can use a locale to internationalize how it shows the type
- .For example, you can insert a double into the cout object and the value will be printed to the console. 
- The default locale, American English, uses the period to separate whole numbers from the fractional part, but in other cultures a comma is used.



The imbue function will change the localization until the method is called subsequently:

```c++
    cout.imbue(locale("american")); 
    cout << 1.1 << "n"; 
    cout.imbue(locale("french")); 
    cout << 1.1 << "n"; 
    cout.imbue(locale::classic());
```

- Here, the stream object is localized to US English and then the floating-point number 1.1 is printed on the console. Next, the localization is changed to French, and this time the console will show 1,1. In French, the decimal point is the comma. The last line resets the stream object by passing the locale returned from the static classic method. This returns the so-called **C locale**, which is the default in C and C++ and is American English.

- The static method global can be used to set the locale that will be used as the default by each stream object. When an object is created from a stream class it calls the locale::global method to get the default locale. The stream clones this object so that it has its own copy independent of any local subsequently set by calling the global method. Note that the cin and cout stream objects are created before the main function is called, and these objects will use the default C locale until you imbue another locale. However, it is important to point out that, once a stream has been created, the global method has no effect on the stream, and imbue is the only way to change the locale used by the stream.

- The global method will also call the C setlocale function to change the locale used by the C Runtime Library functions. This is important because some of the C++ functions (for example to_string, stod, as explained in the following text) will use the C Runtime Library functions to convert values. However, the C Runtime Library knows nothing about the C++ Standard Library, so calling the C setlocale function to change the default locale will not affect subsequently created stream objects.

- It is worth pointing out that the **basic_string** class compares **strings** using the **character traits** class indicated by a **template parameter**. The string class uses the **char_traits** class and its version of the compare method does a straight comparison of the corresponding characters in the two strings. This comparison does not take into account cultural rules for comparing characters. If you want to do a comparison that uses cultural rules, you can do this through the collate facet:

https://stackoverflow.com/questions/5319770/what-is-the-point-of-stl-character-traits

```c++
    int compare( 
       const string& lhs, const string& rhs, const locale& loc) 
    { 
        const collate<char>& fac = use_facet<collate<char>>(loc); 
        return fac.compare( 
            &lhs[0], &lhs[0] + lhs.size(), &rhs[0], &rhs[0] + rhs.size()); 
    }
```

## Converting Strings to Numbers

- `stod` / `stoi` convert **C++** string object to **Numeric value**
- `stod` converts to**double**, **integer** for `stoi`.

```c++
    double d = stod("10.5"); 
    d *= 4; 
    cout << d << "n"; // 42
```

- This **initializes** the *floating point variable* **d** with a value of **10.5**
- Provide a **pointer to size_t** variable
  - Which is **initialized** to the **location** of the **first character** that *cannot be converted*

```c++
    string str = "49.5 red balloons"; 

	// init with a value of 4, indicating space between 5 and r is the first character that cannot be 
	// converted to a double.
    size_t idx = 0; 
// take in the number converting the string
    double d = stod(str, &idx); 
    d *= 2; 
    string rest = str.substr(idx); 
    cout << d << rest << "n"; // 99 red balloons
```

## Converting numbers to strings

- The <string> library provides various overloads of the **to_string** function to convert integer types and floating point types into a string object.
-  This function **does not allow** you to provide any **formatting details**, so for an **integer you cannot indicate the radix** of the string representation (for example, **hex**), and for **floating point conversions**, you have **no control over options like the number of significant figures**. 
- The **to_string** function is a **simple function with limited facilities**. A better option is to use the **stream classes**, as **explained** in the **following** section.

## Using stream classes

- You can **print floating point numbers** and **integers** to the **console** using the **cout object** (an instance of the **ostream class**) or to **files** with an instance of **ofstream.** 
- Both of these classes will **convert numbers to strings** using **member methods** and **manipulators** to affect the **formatting** of the **output string.** 
- Similarly, the **cin object** (an instance of the **istream class**) and the **ifstream class** can **read data** from **formatted streams**.

- Manipulators are **functions** that **take a reference to a stream** object and **return that reference**. The Standard Library has various global **insertion operators** whose parameters are a **reference to a stream object** and a **function pointe**r. 

---

- The appropriate **insertion operator** will call the **function pointer** with the **stream object as its parameter**. 
- This means that the **manipulator will have access to**, and can **manipulate**, the stream it is **inserted into**. For **input streams**, there are also **extraction operators** that have a **function parameter** which will call the **function** with the **stream object**.

- The architecture of C++ streams means that there is a **buffer between the stream interface** that you **call in your code** and the **low-level infrastructure** that **obtains the data.** 

---

- The C++ Standard Library provides **stream classes** that have **string objects as the buffer**. 
- For an output stream, you **access the string after items have been inserted in the stream**, which means that the string will contain those **items formatted** according to **those insertion operators**. 
- Similarly, you can **provide a string with formatted data** as the **buffer for an input stream**, and when you use **extraction operators** to **extract the data** from the stream you are **actually parsing the string and converting parts** of the string to numbers.

- In addition, **stream classes have a locale object** and stream objects will call the **conversion facet** of this **locale to convert character** sequences from one encoding to another.

---

## Outputting floating point numbers

- `<ios>` library has **manipulators** to alter how **streams** handle numbers
-  Default output in **decimal format** for numbers `0.001` to `100,000` 
- For numbers out of this range $\to$ use a **scientific format** with a **mantissa** and **exponent**
- Mixed format $\to$ default behaviour of `defaultFloat` **manipulator**
- Insert **scientific manipulator** into the **output stream** to always use scientific notation
- use **fixed** manipulator $\to$ to obtain a fractional bit on the right and left side.

```c++
    double d = 123456789.987654321; 
    cout << d << "n"; 
    cout << fixed; 
    cout << d << "n"; 
    cout.precision(9); 
    cout << d << "n"; 
    cout << scientific; 
    cout << d << "n";
```

- output

```c++
    1.23457e+08 // large number notation
    123456789.987654 // default fixed behaviour 6 dp
    123456789.987654328 // precision to 9 dp, also use set precision in iomanip
    1.234567900e+08 // switched to scientific format with 9 dp to the mantissa
```

- Default is that the **exponent** is identified by **lowercase** `e` 
- `uppercase` to make them **uppercase**, lowercase with **nouppercase**.

- The way the **fractional parts** are stored means that **in fixed formats** with **9 decimal places** > see that the ninth digit is **8** rather than **1** as you **expected**

---

- Can specify whether a **+** symbol is **shown for a positive**

- `showpos` to display this symbol.
- `showpoint` ensure the decimal point is **shown** even if the floating point number is whole
- `noshowpoint` if there is no fractional part , no decimal point displayed
- `setw` used with both integer/ floating point 
  - Defines **min width** of the space

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210722193553816.png)

