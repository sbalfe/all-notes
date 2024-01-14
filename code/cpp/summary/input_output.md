# C++ I/O

### Introduction

- **C++ I/O Overview:**
  - I/O functionality in C++ comes from the standard library, not the core language.
  - I/O operations are part of the `std` namespace.
- **iostream Library:**
  - Accessed by including the `iostream` header.
  - Contains a hierarchy of I/O classes.
  - Employs multiple inheritance, but is designed to avoid related issues.
- **Concept of Streams:**
  - Core of C++ I/O, represented as a sequential byte sequence.
  - Two main types:
    - Input streams: Receive data from sources like keyboards, files, networks.
    - Output streams: Send data to destinations like monitors, files, printers.
- **Input/Output Classes:**
  - `ios`: Typedef for shared input/output functionalities.
  - `istream`: Handles input streams using the extraction operator (`>>`).
  - `ostream`: Manages output streams with the insertion operator (`<<`).
  - `iostream`: Facilitates bidirectional I/O.
- **Standard Streams:**
  - Pre-defined and environment-provided streams in C++.
  - Includes:
    - `cin`: `istream` for standard input (usually the keyboard).
    - `cout`: `ostream` for standard output (usually the monitor).
    - `cerr`: `ostream` for unbuffered standard error output.
    - `clog`: `ostream` for buffered standard error output.
- **Buffered vs Unbuffered Output:**
  - Unbuffered output is processed immediately.
  - Buffered output is stored and sent out as a block.
- **Future Topics:**
  - Further exploration of I/O functionalities in upcoming lessons.

### Basic input (istream)

```cpp
#include <iostream>
#include <iomanip>  // For std::setw
#include <string>   // For std::string and std::getline

int main() {
    // Using extraction operator (>>) for built-in data types
    int num;
    std::cin >> num; // Reads an integer from the user

    // Avoiding Buffer Overflow
    char buf[10];
    std::cin >> std::setw(10) >> buf; // Limits input to 9 characters (+1 for null terminator)

    // Extraction and Whitespace
    // Reading character by character, skipping whitespace
    char ch;
    while (std::cin >> ch)
        std::cout << ch;

    // Reading input with whitespace using get()
    char ch2;
    while (std::cin.get(ch2))
        std::cout << ch2;

    // Reading strings using get() with character limit
    char strBuf[11];
    std::cin.get(strBuf, 11);
    std::cout << strBuf << '\n';

    // Handling newline characters with get()
    std::cin.get(strBuf, 11); // Reads up to 10 characters, stops at newline
    std::cout << strBuf << '\n';
    std::cin.get(strBuf, 11); // Tries to read again, but stops if newline is first character

    // Using getline() to read strings (and discard newline)
    std::cin.getline(strBuf, 11);
    std::cout << strBuf << '\n';

    // Using getline() for std::string
    std::string str;
    std::getline(std::cin, str);
    std::cout << str << '\n';

    // Additional istream functions
    std::cin.ignore();  // Ignores the next character
    std::cin.ignore(5); // Ignores the next 5 characters
    std::cin.peek();    // Reads next character without extracting it
    std::cin.unget();   // Puts the last read character back into the stream
    std::cin.putback('a'); // Puts 'a' back into the stream to be read again

    return 0;
}
```

### Basic output (ostream)

```cpp
#include <iostream>
#include <iomanip> // For manipulators like std::setprecision, std::setw, std::setfill

int main() {
    // Using the insertion operator (<<) to output information
    std::cout << "Example text" << '\n';

    // Flags and Manipulators for Formatting
    // Example of turning on a flag
    std::cout.setf(std::ios::showpos); // Turns on the showpos flag
    std::cout << 27 << '\n'; // Outputs: +27

    // Turning on multiple flags using Bitwise OR
    std::cout.setf(std::ios::showpos | std::ios::uppercase);
    std::cout << 1234567.89f << '\n'; // Outputs: +1.23457E+06

    // Turning off a flag
    std::cout.unsetf(std::ios::showpos);
    std::cout << 28 << '\n'; // Outputs: 28

    // Handling mutually exclusive flags
    std::cout.setf(std::ios::hex, std::ios::basefield); // Turning on hex, turning off other basefield flags
    std::cout << 27 << '\n'; // Outputs: 1b

    // Using Manipulators
    std::cout << std::hex << 27 << '\n' // Outputs: 1b
              << std::dec << 28 << '\n'; // Outputs: 28

    // Boolean formatting with manipulators
    std::cout << std::boolalpha << true << ' ' << false << '\n'; // Outputs: true false

    // Numeric formatting
    std::cout << std::showpos
              << std::fixed << std::setprecision(2) << 123.456 << '\n'; // Outputs: +123.46

    // Width, Fill Characters, and Justification
    std::cout.fill('*');
    std::cout << std::setw(10) << std::right << -12345 << '\n'; // Outputs: ****-12345

    // Resetting to default settings
    std::cout.fill(' ');
    std::cout.unsetf(std::ios::showpos | std::ios::fixed);
    std::cout << 12345 << '\n'; // Outputs: 12345

       // Floating Point Precision and Notation
    std::cout << std::fixed << std::setprecision(3) << 123.456789 << '\n';  // Outputs: 123.457
    std::cout << std::scientific << 123.456789 << '\n';                    // Outputs: 1.234e+002

    // Integral Base Formatting
    std::cout << std::hex << 27 << '\n';       // Outputs: 1b (hexadecimal)
    std::cout << std::oct << 27 << '\n';       // Outputs: 33 (octal)
    std::cout << std::dec << 27 << '\n';       // Outputs: 27 (decimal)

    // Boolean Formatting
    std::cout << std::boolalpha << true << '\n';    // Outputs: true
    std::cout << std::noboolalpha << false << '\n'; // Outputs: 0

    // Show Positive Sign
    std::cout << std::showpos << 27 << '\n'; // Outputs: +27
    std::cout << std::noshowpos << 28 << '\n'; // Outputs: 28

    // Uppercase Formatting for Hexadecimal and Scientific Notation
    std::cout << std::uppercase << std::hex << 27 << '\n'; // Outputs: 1B
    std::cout << std::scientific << 123.456789 << '\n';    // Outputs: 1.234E+002

    // Width and Fill Characters
    std::cout << std::setfill('-') << std::setw(10) << -12345 << '\n'; // Outputs: -----12345

    // Justification
    std::cout << std::right << std::setw(10) << -12345 << '\n';    // Right-justified: -----12345
    std::cout << std::left << std::setw(10) << -12345 << '\n';     // Left-justified: -12345-----
    std::cout << std::internal << std::setw(10) << -12345 << '\n'; // Internal-justified: -    12345

    // Resetting stream to default state
    std::cout.fill(' ');
    std::cout.unsetf(std::ios::fixed | std::ios::scientific | std::ios::hex | std::ios::oct | std::ios::showpos | 	  std::ios::boolalpha | std::ios::uppercase);
    std::cout << std::dec; // Ensure decimal base

    // Printing default formatted number
    std::cout << 123456 << '\n'; // Outputs: 123456

    return 0;
}
```

### Stream classes for strings

```cpp
#include <iostream>
#include <sstream>  // Include for string stream classes

int main() {
    // String Stream Classes
    // - istringstream, ostringstream, stringstream for normal characters
    // - wistringstream, wostringstream, wstringstream for wide characters

    // Insertion into a stringstream
    std::stringstream ss;
    ss << "en garde!\n"; // Insert string into the stringstream

    // Setting the value of the buffer directly
    std::stringstream ssDirect;
    ssDirect.str("en garde!"); // Set the buffer to "en garde!"

    // Retrieving data from a stringstream
    // Using str() function
    std::stringstream ssRetrieve;
    ssRetrieve << "12345 67.89\n";
    std::cout << ssRetrieve.str(); // Outputs: 12345 67.89

    // Using extraction operator
    std::stringstream ssExtract;
    ssExtract << "12345 67.89"; // Insert string of numbers into the stream

    std::string strValue1, strValue2;
    ssExtract >> strValue1 >> strValue2; // Extract values
    std::cout << strValue1 << " - " << strValue2 << '\n'; // Outputs: 12345 - 67.89

    // Conversion between strings and numbers
    // Converting numbers to strings
    std::stringstream ssConvert;
    int nValue = 12345;
    double dValue = 67.89;
    ssConvert << nValue << ' ' << dValue;
    std::cout << ssConvert.str() << '\n'; // Outputs: 12345 67.89

    // Converting strings to numbers
    std::stringstream ssConvertBack;
    ssConvertBack << "12345 67.89";
    ssConvertBack >> nValue >> dValue;
    std::cout << nValue << ' ' << dValue << '\n'; // Outputs: 12345 67.89

    // Clearing a stringstream for reuse
    std::stringstream ssClear;
    ssClear << "Hello ";
    ssClear.str(""); // Erase the buffer
    ssClear.clear(); // Reset error flags
    ssClear << "World!";
    std::cout << ssClear.str(); // Outputs: World!

    return 0;
}
```

### Stream states and input validation

```cpp
#include <iostream>
#include <sstream>  // Include for string stream classes

int main() {
    // String Stream Classes
    // - istringstream, ostringstream, stringstream for normal characters
    // - wistringstream, wostringstream, wstringstream for wide characters

    // Insertion into a stringstream
    std::stringstream ss;
    ss << "en garde!\n"; // Insert string into the stringstream

    // Setting the value of the buffer directly
    std::stringstream ssDirect;
    ssDirect.str("en garde!"); // Set the buffer to "en garde!"

    // Retrieving data from a stringstream
    // Using str() function
    std::stringstream ssRetrieve;
    ssRetrieve << "12345 67.89\n";
    std::cout << ssRetrieve.str(); // Outputs: 12345 67.89

    // Using extraction operator
    std::stringstream ssExtract;
    ssExtract << "12345 67.89"; // Insert string of numbers into the stream

    std::string strValue1, strValue2;
    ssExtract >> strValue1 >> strValue2; // Extract values
    std::cout << strValue1 << " - " << strValue2 << '\n'; // Outputs: 12345 - 67.89

    // Conversion between strings and numbers
    // Converting numbers to strings
    std::stringstream ssConvert;
    int nValue = 12345;
    double dValue = 67.89;
    ssConvert << nValue << ' ' << dValue;
    std::cout << ssConvert.str() << '\n'; // Outputs: 12345 67.89

    // Converting strings to numbers
    std::stringstream ssConvertBack;
    ssConvertBack << "12345 67.89";
    ssConvertBack >> nValue >> dValue;
    std::cout << nValue << ' ' << dValue << '\n'; // Outputs: 12345 67.89

    // Clearing a stringstream for reuse
    std::stringstream ssClear;
    ssClear << "Hello ";
    ssClear.str(""); // Erase the buffer
    ssClear.clear(); // Reset error flags
    ssClear << "World!";
    std::cout << ssClear.str(); // Outputs: World!

    return 0;
}
```

### Basic file I/O

```cpp
#include <fstream>
#include <iostream>
#include <string>

int main() {
    // File Output using ofstream
    std::ofstream outFile("Sample.txt"); // Open a file for writing
    if (!outFile) {
        std::cerr << "Cannot open file for writing.\n";
        return 1;
    }
    outFile << "This is line 1\n"; // Write to the file
    outFile << "This is line 2\n";
    // File is automatically closed when outFile goes out of scope

    // File Input using ifstream
    std::ifstream inFile("Sample.txt"); // Open a file for reading
    if (!inFile) {
        std::cerr << "Cannot open file for reading.\n";
        return 1;
    }
    std::string line;
    while (std::getline(inFile, line)) { // Read lines from the file
        std::cout << line << '\n';
    }
    // File is automatically closed when inFile goes out of scope

    // Append to file using ofstream with append mode
    std::ofstream appendFile("Sample.txt", std::ios::app); // Open in append mode
    if (!appendFile) {
        std::cerr << "Cannot open file for appending.\n";
        return 1;
    }
    appendFile << "This is line 3\n"; // Append to the file
    appendFile << "This is line 4\n";

    // Explicitly opening and closing files
    std::ofstream explicitFile;
    explicitFile.open("Sample.txt", std::ios::app); // Explicitly open a file
    if (!explicitFile) {
        std::cerr << "Cannot open file explicitly.\n";
        return 1;
    }
    explicitFile << "This is an explicitly added line\n";
    explicitFile.close(); // Explicitly close the file

    return 0;
}
```

### Misc I/O

```cpp
#include <fstream>
#include <iostream>
#include <string>

int main() {
    // Using file pointer with seekg() and seekp()
    std::fstream fileStream("Sample.txt", std::ios::in | std::ios::out | std::ios::binary);

    if (!fileStream) {
        std::cerr << "Unable to open file.\n";
        return 1;
    }

    // Move the file pointer to specific positions
    fileStream.seekg(5); // Move to the 5th byte for reading
    fileStream.seekp(5); // Move to the 5th byte for writing

    // Reading and writing using seekg() and seekp()
    char ch;
    while (fileStream.get(ch)) { // Read one character
        if (ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u') {
            fileStream.seekp(-1, std::ios::cur); // Move back one character for writing
            fileStream.put('#'); // Replace vowel with '#'
            fileStream.seekg(fileStream.tellp()); // Synchronize read and write pointers
        }
    }

    // Demonstrating tellg() and tellp()
    fileStream.seekg(0, std::ios::end);
    std::cout << "File size: " << fileStream.tellg() << " bytes\n";

    fileStream.close();

    return 0;
}
```

