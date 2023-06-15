# Arrays - C++

- Series of **elements** of the **same type** placed in **contiguous memory** locations that are **individually referenced** by adding an **index** to some **unique identifier**.

- Five **values** of *type int* are declared an **array** without **having** to *declare* $5$ different variables (*each with its own identifier*)
  - Rather $\to$ use **an array**
    - The $5$ `int` values are stored in **contiguous memory locations**, and *all five* can be **accessed** using the *same identifier*, with the **proper index**.
- Example $\to$ An **array** *containing* $5$ integer value of type `int` called `foo`:

![image-20220205200316191](D:\University\Notes\DiscreteMaths\Resources\image-20220205200316191.png)

- Each **blank panel** represents **some element** of the **array**
  - The values are **of type** `int` for this case

---

- Like a **regular variable**
  - An **array** must **be declared** *before* it is used
    - A **typical declaration** for *an array* in $C++$

$$
\text{type name[elements];}
$$

- Whose *type* is a **valid** type such as **int / float** 
  - Is some **valid identifier** and the `elements` field, specifies the **length** of the **array** in terms **of number of elements**

---

```c++
int foo [5];
```

## Initializaing arrays

- Default $\to$ **regular arrays** of *local scope* for example those **declared** *within a function* are left **uninitialized** 
  - Therefore **none** of the **elements** are set to any **particular value** $\to$ **==undetermined contents==**. 
- Elements in arrays may be **explicitly initilalized** with *specific values*:

```c++
int foo [5] = {16, 2, 77, 40, 12071};
int foo [5] = {} /* set all to 0*/ 
int foo {10, 20, 30}; /* universal initialisation */
```

---

- Use `std::array` as an alternative.
