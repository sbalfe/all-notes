# Chapter 3 - A Minimal Project

- Every project has `CMakeLists.txt` placed in the **top** of the **source tree**.
  - Defines everything about the build from sources / targets > testing / packaging / custom tasks.

---

- Cmake defines its **own language** with common concepts.
- For now we obtain a **simple build** working as a **starting point**.

```cmake
cmake_minimum_required(VERSION 3.2)
project(MyApp)
add_executable(MyExe main.cpp)
```

- Examples of CMake commands.

```cmake
add_executable(MyExe
  main.cpp
  src1.cpp
  src2.cpp
)
```

- Command names are **case insensitive** 

```cmake
# all the same 

add_executable(MyExe main.cpp)
ADD_EXECUTABLE(MyExe main.cpp)
Add_Executable(MyExe main.cpp)
```

## Managing CMake Versions

- Define a specific CMake version to make sure it behaves us such.
  - Can fix bugs internally and introduce new features whilst keeping the expected behaviour of any particular past release.
- `cmake_minimum_required` $\to$ if CMake used that is lower $\to$ CMake halts immediately.
  - A *warning* issued if not present.
- `VERSION` keyword must always be present, with at least `major.minor` section

```cmake
cmake_minimum_required(VERSION major.minor[.patch[.tweak]])
```

## Project() Command

- This is required in **every CMake Project** and appears after `cmake_minimum_required` is called

```cmake
project(projectName
  [VERSION major[.minor[.patch[.tweak]]]]
  [LANGUAGES languageName ...]
)
```

- `projectName` is used for **top level** of our project in **some generators** and
  - Used in **other parts of project**
    - Such as to **act as defaults** for *packaging / documentation* metadata.

---

- `VERSION` specifies the project version of our application for use in other places

---

- `LANGUAGES` programming lanuguages enabled for our project, defaults as C CXX.
  - C
  - CXX
  - etc...

---

- Project checks for each language enabled whether they **compile / link successfully**. 
- CMake then sets up **many variables** and **properties** to control the **build** for the **enabled language**.

---

- When the compiler and linker checks are performed and *successful* 

  - Results are **cached**
    - Stored in the **build directory** of `CMakeCache.txt` file
      - Additional detail

## Building a basic executable file

- `add_executable()` tells **CMake** to *create* an *executable* from a **set of source files**
  - Basic form **of this command**.

```cmake
add_executable(targetName source1 [source2 ...])
```

- Creates an **executable** referred to within the **CMake** project as `targetName`. 
- Project **built** $\to$ an **executable** created in the **build directory** with a *platform dependent name* $\to$ the *default name* being based on the **target name**.
- Consider the **following simple example command**

```cmake
add_executable(MyApp main.cpp)
```

## Comments

```cmake
cmake_minimum_required(VERSION 3.2)

# We don't use the C++ compiler, so don't let project()
# test for it in case the platform doesn't have one
project(MyApp VERSION 4.7.2 LANGUAGES C)

# Primary tool for this project
add_executable(MainTool
  main.c
  debug.c # Optimized away for release builds
)

# Helpful diagnostic tool for development and testing
add_executable(TestTool testTool.c)
```

## Recommmended practices

- Ensure `cmake_minimum_required()` is always at the top.
  - Consider that *later* versions $\to$ the **more CMake** features the *project* will be **able to use**.

