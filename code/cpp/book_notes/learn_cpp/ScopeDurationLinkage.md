# Namespaces

- Can define the **same namespace** to be used within the program

###### Duplicate Namespace names

```c++
#ifdef CIRCLE_H
#define CIRCLE_H

namespace basicMath{
    constexpr double pi {3.14};
}

#endif
```

```c++
#ifndef GROWTH_H
#define GROWTH_H

namespace basicMath
{
    // the constant e is also part of namespace basicMath
    constexpr double e{ 2.7 };
}

#endif
```

```c++
#include "circle.h" // for basicMath::pi
#include "growth.h" // for basicMath::e

#include <iostream>

int main()
{
    std::cout << basicMath::pi << '\n';
    std::cout << basicMath::e << '\n';

    return 0;
}
```

###### Namespace aliases

```c++
#include <iostream>

namespace foo::goo
{
    int add(int x, int y)
    {
        return x + y;
    }
}

int main()
{
    namespace active = foo::goo; // active now refers to foo::goo

    std::cout << active::add(1, 2) << '\n'; // This is really foo::goo::add()

    return 0;
} // The active alias ends here
```

# Local variables

###### Local variables have no linkage

- **==Linkage==** determines whether **other declarations** of this name *refer* to the **same object** or *not*
- Local variables have **not go this** and therefore each **declaration** refers to a **unique object**

```c++
int main()
{
    int x { 2 }; // local variable, no linkage

    {
        int x { 3 }; // this identifier x refers to a different object than the previous x
    }

    return 0;
}
```

###### Variables should be defined in the most limited scope

```c++
#include <iostream>

int main()
{
    // do not define y here

    {
        // y is only used inside this block, so define it here
        int y { 5 };
        std::cout << y << '\n';
    }

    // otherwise y could still be used here, where it's not needed

    return 0;
}
```

# Global variables

- Declared at the **top of the file**, *below* the *includes* but **above any code**
- Example of a **global variable** being defined.

```c++
#include <iostream>

// Variables declared outside of a function are global variables
int g_x {}; // global variable g_x, marked with this naming convention.

void doSomething()
{
    // global variables can be seen and used everywhere in the file
    g_x = 3;
    std::cout << g_x << '\n';
}

int main()
{
    doSomething();
    std::cout << g_x << '\n';

    // global variables can be seen and used everywhere in the file
    g_x = 5;
    std::cout << g_x << '\n';

    return 0;
}
// g_x goes out of scope here
```

###### Global variables have file scope and static duration

- **==File scope==** $\to$ they are visible from the **point of declaration** until the **end of the file** in which they are **declared**.
- Once *declared* $\to$ a **global variable** is used **anywhere** in the **file** from that **point onwards**.
- Global variables are **created** when the **program starts** the *destroyed when it ends* $\to$ **==static duration==** also called **==static variables==**.

###### Global variable initialisation

```c++
int g_x; // no explicit initializer (zero-initialized by default)
int g_y {}; // zero initialized
int g_z { 1 }; // initialized with value
```

###### Constant global variables

- Just as with **local variables**

```c++
#include <iostream>

const int g_x; // error: constant variables must be initialized
constexpr int g_w; // error: constexpr variables must be initialized

const int g_y { 1 };  // const global variable g_y, initialized with a value
constexpr int g_z { 2 }; // constexpr global variable g_z, initialized with a value

void doSomething()
{
    // global variables can be seen and used everywhere in the file
    std::cout << g_y << '\n';
    std::cout << g_z << '\n';
}

int main()
{
    doSomething();

    // global variables can be seen and used everywhere in the file
    std::cout << g_y << '\n';
    std::cout << g_z << '\n';

    return 0;
}
// g_y and g_z goes out of scope here
```

# Variable shadowing