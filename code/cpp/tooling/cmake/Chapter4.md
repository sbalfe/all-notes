# Chapter 4 - Building simple targets

- Easy to define a **basic executable** in `CMake` 

```cmake
add_executable(MyApp main.cpp)
```

- can define more than just a **basic command line** application.
  - May be app bundles on app platform and windows gui applicaitions

## Executables

- Complete form of the **basics** `add_executable()` command is as **follows**

```cmake
add_executable(targetName [WIN32] [MACOSX_BUNDLE]
  [EXCLUDE_FROM_ALL]
  source1 [source2 ...]
)
```

- Only differences to the form **shown before** are the **new optional keywords**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220219231220270.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220824024947789.png)

- Additionally $\to$ there are **other forms** of `add_executable()` produce a **kind of reference** to *an existing executable* or target rather than **defining** a *new one* to **be built**

## Defining libraries

- CMake supports a variety of **different kinds of libraries**
  - Taking care of **many of the platform differences** but still supporting the native idiosyncrasies of each.
    - Library targets are **defined** using `add_library()` command of which there are **number of forms**
- Most basic of these:

```cmake
add_library(targetName [STATIC | SHARED | MODULE]
  [EXCLUDE_FROM_ALL]
  source1 [source2 ...]
)
```

- This is *analgous* to `add_executable()` when defining a **simple executable**
- `targetName` used within `CMakeLists.txt`  to refer to the **library**
  - The **name** of the **built library** on the **filesystem** being **dervied** from this name as *default*
- `EXCLUDE_FROM_ALL` keyword has the **same effect** as `add_executable()` 
  - Prevent library from **being included** in the default `ALL` target
- Type of library **to be built** specified by one of the **keywordsS**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220219232520229.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220219232716407.png)

---

- 