# Using the Standard Library Containers

- Every standard library provides many containers, via a templated class
  - Therefore the behaviour is generic.

    - There are *classes* for *sequential* containers, when ordering of the items in the container, dependent on the *order* that **items are inserted into container**.
- There are sorted/unsorted containers that associate a value with a key

## Pairs and tuples

- Wish to **associate two items** together ^e834cc
  - example $\to$ **associative** container lets you create a **type of array** where **items** other than **numbers** are *used as an index*
- The `<utility>` header contains a **templated class** of `pair`
  - has *first* and *second* data **members**

```c++
    template <typename T1, typename T2> 
    struct pair 
    { 
        T1 first; 
        T2 second; 
        // other members 
    };
```
```c++

```
- can **associate any item** as its **templated**.
  - Can access easy with **dot** or **get** `get<0>(p)` rather than `p.first` 
- Class has a **copy constructor**
  - Can **create** an **object** from **another object**
- Class has a **move constructor** also
  - Function called `make_pair` that will **deduce** the **types** of the *member* from  the **parameter**.

```c++
 auto name_age = make_pair("Richard", 52);
```

- Compiler uses the **type** that **deems** most **appropriate**
  - For this case it will be `pair<const char*, int>`
- If you want the **first** item to be a **string**
  - Just use the **constructor**
    - Can **compare `pair` objects** $\to$ this comparison done on the **first member**, only if they are equal is **second compared**.

```c++
    pair <int, int> a(1, 1); 
    pair <int, int> a(1, 2); 
    cout << boolalpha; 
    cout << a << " < " << b << " " << (a < b) << endl;
```

- The **parameters** can be **references**

```c++
    int i1 = 0, i2 = 0; 
    pair<int&, int&> p(i1, i2); 
    ++p.first; // changes i1
```

- The `make_pair` function **deduces** then *types* from the **parameters**
  - The *compiler* cant tell the *difference* between a **variable** and a **reference** to a **variable**
    - C++11 $\to$ use the **ref function** `<functional>` to specify that the **pair** is for **references**

```c++
    auto p2 = make_pair(ref(i1), ref(i2)); 
    ++p2.first; // changes i1
```

- To **return** *two values* from some **function**
  - achieve via **parameters** passed as **reference**
    - This code is **less readable** as you *expect a return value* to **come via return of function** rather than via the **parameters**

- The `pair` enables you to **return two values** in *one object*
  - Example is `minmax` in `<algorithm>` 
    - Returns a `pair` object *containing* the **parameters** in order of *the smallest first*
      - There is an **overload** where you **can provide** a *predicate* object if the **default operator** `<` should **not be used**
- Prints `{10,20}`

```c++
    auto p = minmax(20,10);  
    cout << "{" << p.first << "," << p.second << "}" << endl;
```

- The `pair` class **associates two items**
  - The **standard library** gives the `tuple` , however its **variadic** therefore **any number of parameters**

- The data members are **not named** in `pair` 
  - Access them **via** the **templated** `get` function

```c++
    tuple<int, int, int> t3 { 1,2,3 }; // create a tuple to hold three items, using initialize list (could use constructor syntax)
    cout << "{" 
        << get<0>(t3) << "," << get<1>(t3) << "," << get<2>(t3)  
        << "}" << endl; // {1,2,3}

// tuple printed to console via accesing each data member in the object using a version of the get function
// where the template parameter indicate the index of the item
```

- The **index** here is a **template parameter**
  - Can *not provide* it at **runtime** using some **variable**
    - If you must then **use** `vector` instead.

https://stackoverflow.com/questions/16424974/access-tuple-element-in-c11-via-compile-time-variable-in-function

- The `get` function returns a **reference**
  - Therefore can **alter value** of the **item**
    - For some `tuple t3` $\to$ this code changes the **first item** to `42` and second to `99`

```c++
    int& tmp = get<0>(t3); 
    tmp = 42; 
    get<1>(t3) = 99;
```

- Can **extract** *every item* with a **single call**
  - Use the **tie function** 

```c++
    int i1, i2, i3; 
    tie(i1, i2, i3) = t3; 
    cout << i1 << "," << i2 << "," << i3 << endl;
```

- The **tie function** returns a **tuple** where *each parameter* is a **reference** and **init** to the **variables** you *pass* as **parameters**
  - The previous code is **easier** to **understand** if *you write like so*

```c++
    tuple<int&, int&, int&> tr3 = tie(i1, i2, i3); 
    tr3 = t3;
```

- `tuple` object can be made from a `pair` object
  - Can use `tie` function to **extract values** from a `pair` object too.
- Helper function called `make_tuple` 
  - Will **deduce** the **types** of the **parameters**
    - be wary of **deductions**
      - A *floating point number* is **deduced** to a **double** and an **integer** will be **int**
- Make them references to **specific variables**
  - Can use `ref` function or `cref` function for **const reference**

- Can **compare** `tuple` objects provided there are **equal number** of **items** with *equivalent types*
  - The *compiler* will **refuse** to **compile comparison** of **tuple objects** that *have different number of items*
    - Or if the **types** of the *items* of a `tuple` can **not be converted** *implicitly* to the other `tuple object` . 

## Containers

- Can group zero or more items of **same type** and access them **serially** via **iterators**
  - Each one has a **begin method** to return *iterator* to the **first item**
  - Each one also has **end method** to *return iterator* to **after the last item** in the *container.* 
- The **iterator** object *supports* **pointer arithmetic**
  - Thus `end() - begin()` obtains the **number of items** in the *container*
    - All **container types** will **implement** the **empty method** to indicate there are **no items** in the *container*
    - Also, apart from **forward_list**, a **size method** for the **number of items** in *container*
      - BEFORE > always iterated through container like a **an array**

```c++
  vector<int> primes{1, 3, 5, 7, 11, 13}; 
    for (size_t idx = 0; idx < primes.size(); ++idx)  
    { 
        cout << primes[idx] << " "; 
    } 
    cout << endl;
```

- **not all** containers **allow random access**
  - Deciding on **changing** to *other containers* for **efficiency**
    - must **change** how you **access**
- Code also **does not work** if you want to write **generic code** in **templates**
  - Previous code $\to$ better using **iterators**.

```c++
   template<typename container> void print(container& items) 
    { 
        for (container::iterator it = items.begin();  it != items.end(); ++it) 
        { 
            cout << *it << " "; 
        } 
        cout << endl; 
    }
```

- Every container $\to$ has a `typedef` member called **iterator**
  - Gives the **type** of the **iterator** returned from **begin** method.
- *Iterator objects* are **like pointers**
  - Can obtain the **item** an **iterator** refers to using **dereference** and then *move* to the **next item** using the **increment operator**
- All containers **apart** from **vector**
  - There is a **guarantee** that an *iterator* remains **valid** , even if the other elements are **deleted**

---

- All containers $\to$ have an **exception safe** *no throw* method of `swap` 
  - With **two exceptions** in place $\to$ must have **transactional semantics** (succeed/ fail)
    - If fail $\to$ container is in the **same state** as *before operation* is called
- For every container, this **rule** is **relaxed** for **multi element inserts**
  - If you *insert many items* at a time using **iterator range**
    - Example , and the **iterator** FAILS for *one* of the items in range $\to$ method **will not be able** to *undo previous inserts*.

- Objects are **copied into containers**
  - The *type* of the object must have a **copy constructor/ assignment operator**
- Aware $\to$ if you place a **derived class** object into a **container** that requires its **bass class** object
  - Then **copying** it will **slice the object**
    - Meaning that **anything** to *do with* the **derived class** is *removed* (data methods/ virtual method pointers.) https://www.geeksforgeeks.org/virtual-function-cpp/

## Sequence containers

- Store a **series of items** and the **order** that  they **stored in**
  - Along with **when** you **access them** with an **iterator**
    - The items are **retrieved** in the order which they are placed in the container
      - After creating a container $\to$ can change the sort order with library functions.

### List

- A `list` object **implemented** via a **doubly linked list**
  - Where *each item* has a **link** to the **next / previous one**
    - Thus is **fast insertion**
- There is **no random access** however (no [])

---

- Allows you to **provide** values via some **constructor**
  - Can use **member methods** also.
- `assign` member method $\to$ allows you to **fill the container** in *one action* using **init list**
  - Alternatively use **iterators** , to a **range** of another *container*
- Insert a **single item** using the `push_back` or `push_front` method.

```c++
    list<int> primes{ 3,5,7 }; // create a list of 3,5,7 
    primes.push_back(11); // push this to back 
    primes.push_back(13); 
    primes.push_front(2); 
    primes.push_front(1);
```

- the methods `pop_back/front` do **not return** actually , just **remove**.

``` c++
    int last = primes.back(); // get the last item, must access this to obtain item before you pop it out the back.
    primes.pop_back();        // remove it
```

- The *clear* method **removes all items** in the *list* 
- The *erase* method **delete items**
- There are *two versions*
  - One has **iterator** that identifies a **single item** and another that has **two iterators** that *indicate a range*
    - A **range** is indicated via **first item** in the **range** and the **item after the range**

```c++
    auto start = primes.begin(); // 1 
    start++;                     // 2 
    auto last = start;           // 2 
    last++;                      // 3 
    last++;                      // 5 
    primes.erase(start, last);   // remove 2 and 3, last does not get removed ofc.
```

- This is **a general principle** with *iterators* and the **standard library** containers
  - A *range* is **indicate** via *iterators* by the **first item** and the **item after** the **last item**
    - The *remove* method will **remove** all items with a **specified value**

```c++
    list<int> planck{ 6,6,2,6,0,7,0,0,4,0 }; 
    planck.remove(6);            // {2,0,7,0,0,4,0}
```

- There is the method `remove_if` that takes some **predicate**
  - Only remove if **predicate** returns true.
    - Similarly $\to$ can **insert items** into a *list* with an **iterator** , then the item is **inserted BEFORE** the *specified item*

```c++
    list<int> planck{ 6,6,2,6,0,7,0,0,4,0 }; 
    auto it = planck.begin(); 
    ++it; 
    ++it; 
    planck.insert(it, -1); // {6,6,-1,2,6,0,7,0,0,4,0}
```

- Can indicate that the item should be **inserted more than once** at *that position*
  - If so $\to$ **how many copies**
    - Can *provide* *several items* to be **inserted** at **one point**.
- If the **iterator** you *pass* is **obtained** by calling the **begin method**
  - The item is **inserted** at the **start** of the *list.* 
    - same as using `push_back`
- When calling `insert` method
  - Provide an **object** that is either **copied** into the list or **moved (r value semantics)**
    - The *class* provides **emplace methods** which *construct* a **new object** based on the **data given** then **insert that**
- Example of **point class** created from **two double values**
  - Either insert a **constructed point** or **emplace** a point with **two double values**

```c++
    struct point 
    { 
        double x = 0, y = 0; 
        point(double _x, double _y) : x(_x), y(_y) {} 
    }; 

    list<point> points; 
    point p(1.0, 1.0); 
    points.push_back(p); 
    points.emplace_back(2.0, 2.0);
```

- Once created a **list**
  - Can *manipulate* with **member functions**
    - The `swap` method takes a **suitable** `list` object as **parameter**
      - it moves the items from the **parameter** into the **current object**
        - Then moves the items in the **current** `list` to the **parameter**
          - As the `list` object is **implemented** via **linked list** $\to$ very fast operation.

```
    list<int> num1 { 2,7,1,8,2,8 }; // digits of Euler's number 
    list<int> num2 { 3,1,4,5,6,8 }; // digits of pi 
    num1.swap(num2);
```

- After this code  the values are swapped

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210710154050842.png)

- A **list** will **hold** the items in the **order** they are **inserted**
  - Can **sort** via calling the `sort` method 
    - By default $\to$ order items **in ascending order** using the `<` operator for **the items** in the **list container**
- Can pass a **function object** for *comparison operation*
  - Once sorted $\to$ can **reverse** the *order* of items via calling **reverse method**
    - Two **sorted lists** can be **merged** $\to$ involves taking the **items** from the **argument list** and **inserting** them into the **calling list**, in order:

```c++
    list<int> num1 { 2,7,1,8,2,8 }; // digits of Euler's number 
    list<int> num2 { 3,1,4,5,6,8 }; // digits of pi 
    num1.sort();                    // {1,2,2,7,8,8} 
    num2.sort();                    // {1,3,4,5,6,8} 
    num1.merge(num2);               // {1,1,2,2,3,4,5,6,7,8,8,8}
```

- Merging **two lists** may **results in duplicates**
  - removed via calling the **unique method**.

```c++
    num1.unique(); // {1,2,3,4,5,6,7,8}
```

### Forward list

- Like the **list** but only **allows** items to **insert** and **remove items** from the *front*.
  - Iterators can **only be incremented**.
  - Compiler simply **refused to decrement**.
- Class has many same methods from list
  - `push_front`
  - `pop_front`
  - `emplace_front`
- Does not have the **back version** of them.

- Insertions occur **after** some **existing item**
  - Therefore implements `inserts_after` and `emplace_after` 

---

- Can **remove items** at the **start** of the list `pop_front` 
  - Or **after** a **specified** item `erase_after` 
- Tell the class to **iterate** in a **forward direction** through the list and **remove items** with a **specific value**
  - `remove` and `remove_if` 

```c++
    forward_list<int> euler { 2,7,1,8,2,8 }; 
    euler.push_front(-1);       // { -1,2,7,1,8,2,8 } 
    auto it = euler.begin();    // iterator points to -1 
    euler.insert_after(it, -2); // { -1,-2,2,7,1,8,2,8 } 
    euler.pop_front();          // { -2,2,7,1,8,2,8 } 
    euler.remove_if([](int i){return i < 0;}); 
                                // { 2,7,1,8,2,8 }
```

- `euler` init with the **digits** of **Euler's numbers**
  - Value of `-1` pushed to the front
    - An **iterator** is **obtained** that *points* to the **first value** in the **container.**
      - This is the `-1` inserted.
- value of --2 inserted after -1
- last two lines $\to$ show how **to remove items** 
  - `pop_front` removes item at front
- `remove_if` $\to$ remove items that **satisfy** the **predicate**.
  - In this case $\to$ when the **item** is **less than zero**.

### Vector

- Behaves like a **dynamic array**
- *indexed **random access*** to items and the **container** can *grow* as **more items** are *inserted into it*
  - Create a **vector** object with an **initialization** list
    - With the **specified** number of **copies** of *an item*
- Can **base** the vector on **values** of *another container*
  - Pass **iterators** that **indicate** the *range* of the **items** in **that container.**
- Create a **vector** with **pre-determined** size by giving some **capacity** as the **constructor parameter**
  - Along with the **specified number** of *default items*.
- If you wish to **specify container** size 
  - Call `reserve` to **specify** the **minimum size** of the **resize method**
    - May mean *deleting excess items* or **creating new items** depending on whether the **existing** *vector* object is **bigger / smaller** than the **requested size**

---

- When you **insert items** into a **vector container** and there is **not enough memory** allocated
  - The container **will allocate enough memory**
    - allocates **new memory**
    -  *Copies* the **existing items** to **new memory**.
    - Creates the **new item**
    - *Destroys* the **old copy** of the **items**
    - *deallocates memory*.

- If you **know** the **number of items**
  - and you know the **vector** container will **not be able to contain** *without new allocation*
    - Indicate **how much space** $\to$ by calling the **reserve method**.

---

- Inserting items **other** than **constructor is straightforward**
  - can use `push_back` to **insert** an **item** at the *end*
    - Which is a **fast action** $\to$ assuming **no allocation** is **needed**.
- There is also `pop_back` to **remove the last item**
  - Can use **assign** method to **clear** the **entire container** and **insert** to **specified items**
    - Either a **multiple** of the **same item**, an *initializer* list of items or **items** in **another container** specified **with iterators**.

- As with **list objects**
  - Can **clear entire vector** / **erase** items at a **position** / **insert** at a **specified position** 
    - There is **no equivalent** of the **remove** method to **remove items** with a **specific value**.

---

- The **main reason** to use *vector* is to obtain **random access** using
  1. **at method**
  2. **[ ] indexing operator**

---

```c++
   vector<int> distrib(10); // ten intervals 
   for (int count = 0; count < 1000; ++count) 
   { 
      int val = rand() % 10; 
      ++distrib[val]; 
   } 
   for (int i : distrib) cout << i << endl;
```

- First line $\to$ creates a **vector** with *ten items*
  - In the loop $\to$ **C runtime function** of *rand* is called a **thousand times** to get a *pseudorandom* number between **0** and **32767**
    - The **mod operator** used to obtain a random number between **0 and 9**
- This random number used as **index** for the **distrib** vector
  - which is then **incremented**
    - The distribution is then printed out.
      - obtain **roughly 100** in **each item**

---

- Code relies on the idea that **[]** returns a **reference** to the **item**
  - Which is why it **can be incremented** in this *manner*
    - The `[]` operator used to **read / write** to some item in **container**
- Container gives **iterator access** via the **begin** and **end** methods, and **front / back methods**
  - As they are **needed** by the **container adapters** (begin and end)

---

- A **vector** object **can hold any type** that has a **copy constructor** and **assignment operator**
  - Therefore **all built in types**
    - A **vector** of **bool** items is a **waste of memory** as *boolean value* can be **stored as** a **single bit** and compile treats the **bool** as an **integer** (*32 bits *)

- The **standard library** has a **specialization** of the *vector* class for **bool** that **stores more efficiently**
  - looks okay $\to$ issue is that as **it holds** **boolean values** as **bits** $\to$ means the `[]` operator **doesnt return** a **reference** to a *bool*
    - Just returns an **object** that **behaves** like one

- To **hold boolean values** and **manipulate** them
  - As long as you **know** at **compile** time **how many items** there are
    - The **bitset** class is **probably better**.

https://www.geeksforgeeks.org/c-bitset-and-its-application/

---

### Deque

- means **double ended queue**
  - Therefore can **grow** from **both ends**
- You can **insert** items in the **middle**, however **more expensive**
- Queue $\to$ means the items are **organised**
  - As they can enter from **either end** $\to$ may not be **in** the **order** that you **put items** into the **container**

---

- Has **iterator access** 
- Has **random access** via `at` and `[]`
- Access items at end via `push_back, pop_back and back` 
  - Unlike a **vector** $\to$ can access **front** of a **deque** container via `push_front, pop_front, front` methods

---

- Has methods of **erase / resize / insert** , however they are **expensive**.
  - Should **reconsider** container if you **need to use**.

- Does **not have methods** to *pre-allocate memory*
  - Add > **may cause memory allocation**.

---

## Associative containers

- C like array / vector $\to$ every item **associated** with its **numeric index**
- This was **exploited earlier** in *vector* where the index provided the **decile** of **distribution** and then it was split into ten deciles of data .

---

- An **==associative container==** allows you to **provide indexes** that are *non numeric*
  - They are **keys** that you **associate values** with.
- Inserting the **key value pairs** $\to$ they **become ordered** so the container than **efficiently** access the **value by the key**.
  - This order **should not matter**, access by the **keys** rather than iteration or **random access**
- Implementation $\to$ uses a **binary tree / hash table**
  - Therefore a **quick operation** to *find an item* via its **key**.

---

- Often **expensive** to *insert / erase*
- Key treated as **immutable** therefore **cannot alter** it for an item
- There are **no remove methods** for each one
- There are **erase methods** 
- Containers that **keep items sorted** $\to$ erasing an **item** *could affect* **performance.**

---

- There are **several types** of **associative containers**
  - The main differences are **handling duplicates / level of ordering**
- `map` $\to$ has **key value pairs** sorted by **unique keys**, therefore **duplicates not**
-  allowed
- Duplicate keys $\to$ use `multimap` 
- `set` is just a **map** where the **key** is the **same as the value**, does **not allow duplicates**
- `multiset` does **allow duplicates**

---

- May seem *odd* to have an **associative class** where the **key** is the *same as value*
  - Reason for **including** the *class in this section* , like map class, the set class has **similar interface** to *find a value*
- Similar to map $\to$ set class is **fast at locating.**

---

### Maps and multimaps

- A `map` container stores **two different items**
  1. key 
  2. value
- Will *maintain* a **sorted** ordered via *key*
- A **sorted map** means its **quick** to **locate an item** 
- Can place items into the container via **constructor**
  - Or use **member methods** of *insert / emplace*
- access to items via **iterators**
  - Iterator gives access to a **single value** 
    - Therefore with a **map** $\to$ this will be a **pair object** that has *both* the *key* and *value*

```c++
	map<string, int> people; 
    people.emplace("Washington", 1789); 
    people.emplace("Adams", 1797); 
    people.emplace("Jefferson", 1801); 
    people.emplace("Madison", 1809); 
    people.emplace("Monroe", 1817); 

    auto it = people.begin(); 
    pair<string, int> first_item = *it; 
    cout << first_item.first << " " << first_item.second << endl;
```

- Call to **emplace** places items **into the map** where the *key* is a **string**, value an **int**
- Obtain **iterator** to the **first item** in the **container**, then the **item** is **accessed** via *dereferencing* the iterator to **obtain** a **pair object**
- Items are **stored** in the **map** *already sorted*
  - First item set to **Adams** 
    - Can *insert* as **pair objects** either via **objects** or through **iterators** to *pair objects* in **another container** using **insert method**
- Most **emplace / insert** return a **pair object** of the *form*
  - Where the **iterator type** is relevant to the map:

```c++
pair<iterator, bool>
```

- Can **use this object** to *test for two things*
  1. **bool** indicates if the **insertion** was **successful** (fail if some item with same key already in container)
  2. **iterator** indicates the **position** of the **new item** 
     - or **indicates** position of **existing item** that will *not be replaced*
       - Causing the insertion to fail

- *Failure* depends on **equivalence** rather than **equality**
  - If there is an **item** with a **key** that is *equivalent* to the **item** you are **trying to insert** $\to$ fails.
- Definition of **equivalence** depends *on* the **comparator** predicate being **used** with the **map object**
  - If a **map** uses a *predicate* *comp* $\to$ then **equivalence** between **two items** `a` and `b` 
    - Determined by **testing** `!comp(a,b) && !comp(b,a)` 
      - This is **not the same** as testing for **a==b**

---

- Assuming **previous map object**
  - You *can do this*:

```c++
    auto result = people.emplace("Adams", 1825); 
    if (!result.second) 
       cout << (*result.first).first << " already in map" << endl;
```

- Second **result variable** tested to see if **insertion successful**
  - If not $\to$ then the **first item** $\to$ is an **iterator** to a `pair<string,int>` 
    - Which is the **existing ** item $\to$ the code then **dereferences** the **iterator** to *get* the *pair* object 
      - Then **prints** out the **first **
      - **item** $\to$ which is the **key** (in this case, the **name of person**).

---

- If you **know** where the **map** the *item* should go
  - Then call **emplace_hint**

```c++
    auto result = people.emplace("Monroe", 1817); 
    people.emplace_hint(result.first, "Polk", 1845);
```

- We know that **polk** comes **after Monroe**
  - Therefore we **pass** the **iterator** to **Monroe** as the **hint**
    - Class gives **access** to **items** via **iterators** , therefore you can **use range for** (based **on iterator access**)

```c++
    for (pair<string, int> p : people) 
    { 
        cout << p.first << " " << p.second << endl; 
    }
```

- Additionally $\to$ there is **access** to **individual** items using the **at / [] ** methods.
  - Both cases $\to$ class **searches for an item** with the **provided key** 
    - if found > a **reference** to the **items value** is **returned**
      - The **at method** and the **[] operator** behave different in a **situation** where there is **no item** with the **specified key**.

---

- If **key does note exist**
  - The **at** method *throws exception*
  - `[]` will **create** a **new item** using the **key** and *calling* **default constructor** of *value type* (int, thus 0)
    - *Returns* a **reference** to *this value* if the **key exists**, write code as such:

```c++
    people["Adams"] = 1825; 
    people["Jackson"] = 1829;
```

- Second **line** *behaves* as expected
  - There is **no item** with a **key** of **Jackson**.
    - Therefore map **creates** an **item** with **that key**, *initialize* it with **default constructor** of *value type* (int, value is **init to zero**).
      - Then **returns** a **reference** to *this value*, which is **assigned** a *value* of *1829*.
- First line , look up to see that **already exists** 
  - Returns a **reference** to *this value*.
    - Assigned **a value** of **1825**.
- There is **no indication** that the value of **some item** has **been changed** as next to **new item** being **inserted**
  - May wish for this behaviour
    - Not the intention here $\to$ clearly an **associative container** that **allows duplicate keys** is *required* 

- For **both cases** $\to$ there is a **search** for the **key** > return reference > assignment performed.
  - Although **valid** to **insert items** this way
    - More efficient to **emplace** a **new key-value pair** in the **container** so there is **not an extra assignment**

---

- After filling the map
  - Search for a **value** using the **following**

| at            | passed a key and a returns a reference to the value for that key |
| ------------- | ------------------------------------------------------------ |
| [ ]           | when **passed a key** returns a **reference** to the *value for that key* |
| *find*        | use the **predicate specified** in the *template* (unlike **global** **find ** function, mentioned later) <br>gives you an **iterator** to the **entire** item as **pair object** |
| *begin*       | Give an **iterator** to the **first item**<br>End method give you an **iterator** *after* the **last item** |
| *lower_bound* | returns an **iterator** to the **item** that has a **key equalto** or **greater** than the **key** you **pass** as a **parameter** |
| *upper_bound* | Method returns **iterator** an **iterator** of the **first item** in the **map** that has a **key greater** than the **key provided** |
| *equal_range* | Method returns both **lower / upper bounds** values in **pair object** |

### Sets and multisets

- Sets act similar to **maps**
  - The **key** is the **same** as the **value** :

```c++
    set<string> people{ 
       "Washington","Adams", "Jefferson","Madison","Monroe",  
       "Adams", "Van Buren","Harrison","Tyler","Polk"}; 
    for (string s : people) cout << s << endl;
```

- Print out **nine people** in *alphabetical order*
  - There are **two items** called **adams**
    - The **set class** will **reject duplicates**  $\to$ 
- As Items are **inserted** into **the set**
  - It will **be ordered**
    - Order **determined** by **lexicon** ordering of **comparing** *two string objects*

- To **enable duplicates** so that **10 can be placed** $\to$ use **multiset**

---

- Like a **map** $\to$ can **not change the key** of an **item** in the **container** as this is **used to determined ordering**
  - For a **set** $\to$ key is **the** *same* as the *value* 
    - Therefore you **cannot change the item** at all
- If intention is **lookups** $\to$ use a `vector` container **if the search** is **sequential**
  - but if you **use a call** to `binary_search` $\to$ could be **faster** than the **associative container**

---

- The **interface** to the `set` class is a **restricted** version of **the map**
  - Can **insert / emplace** 
  - **Assign** it to **values** in **another container**
  - Has **iterator access** (*begin / end*)

---

- There is **no distinct key**
  - The **find method** looks for a **value**, rather than a **key**
    - Similar with the **bounds methods** ; for example `equal_range`
- There is no **at method** , nor an **[ ] operator**

---

## Unordered containers

- The **map / set** enable **fast access**
  - This is because they are **sorted**.
- If you **iterate** through the items **(begin -> end)** 
  - Obtain the **sorted items**.
- Obtain a **selection** of *objects* within a **range** of **key values** 
  - Can **make calls** to the `lower_bound` and `upper_bound` methods to **obtain iterators** to **correct range**

---

- Two main features of **associative containers**
  1. **LOOKUP**
  2. **SORTING**
- Some *cases* the **actual order** of the values is **not important**
  - The **behaviour** you want is **efficient lookup**
    - For this case $\to$ refer to `map` and `set` classes.
      - order not important $\to$ **therefore use a hash table**.

### Special Purpose Containers

- Containers **so far** are **flexible** and can be used for **all kinds of purposes**
  - The *standard library* provides classes that **have specific purposes**.
    - They are **implemented** via **wrapping classes** called **container adapters**
- A `deque` object **can be used** as a **FIFO** queue 
  - by pushing objects to the **back** of the **deque** (`push_back`) 
    - Then just **accessing** from the *front* using the **front method**
      - The *removing* them with `pop_front` method.
- The **standard library** implements a **container adapter** called a **queue**
  - That *has the FIFO* behaviour
    - based on the **deque** class.

```c++
    queue<int> primes; 
    primes.push(1); 
    primes.push(2); 
    primes.push(3); 
    primes.push(5); 
    primes.push(7); 
    primes.push(11); 
    while (primes.size() > 0) 
    { 
        cout << primes.front() << ","; 
        primes.pop(); 
    } 
    cout << endl; // prints 1,2,3,5,7,11
```

- just **push items** into the **queue** and **remove** with `pop`.
  - The **next item** accessed via **front method** 
- The **standard library containers**
  - That *can be wrapped* by **this adapter** ==must implement==
    1. `push_back`
    2. `pop_front`
    3. `front`
- The items are **put** into the **container** at **one end** and **accessed** from the **other**

----

- a **LIFO** container **place the items** and **access** from the **same end**
  - Using a **deque object** can be used to **implement** this **behaviour** by *pushing items* via `push_back`.
- Standard library $\to$ provides an **adaptor class** called **==stack==**
  - This has `push` to place items into the **container**, method called **pop** to **remove items**
    - Access items using the **top method** , even though its **implemented** using the **back method** of some **wrapped container**.

---

- `priority_queue` **adapter class** is used like the **stack container**
  - Items are **accessed** using the **top method**
    - Container *ensures* when some **item is pushed in** $\to$ the *top* is the **highest priority**
- A predicate (default <) can **order the items in the queue**
  - Example $\to$ may have an **aggregate type** that has the **name** of a **task** and **priority**
    - Must complete the task compared to other tasks.

```c++
    struct task 
    { 
    string name; 
    int priority; 
    task(const string& n, int p) : name(n), priority(p) {} 
    bool operator <(const task& rhs) const { 
        return this->priority < rhs.priority; 
        } 
    };
```

- The **aggregate type** is *straightforward*
  - Has **two data members** that are **initialized** via the **constructor**
    - So the **task** can be **ordered**
      - must be able to **compare two task objects**
- Option give  **earlier** is to define a **seperate predicate class**
  - In this example $\to$ use **default predicate**
    - Which the **docs** say is `less<task>`, compares based on **<** operator
      - Use the **default predicate** by defining the **<** for our task class
- Add tasks to it

```c++
    priority_queue<task> to_do; 
    to_do.push(task("tidy desk", 1)); 
    to_do.push(task("check in code", 10)); 
    to_do.push(task("write spec", 8)); 
    to_do.push(task("strategy meeting", 8)); 

    while (to_do.size() > 0) 
    { 
        cout << to_do.top().name << " " << to_do.top().priority << endl; 
        to_do.pop(); 
    }
```

- Result 

```c++
    check in code 10
write spec 8
strategy meeting 8
tidy desk 1
```

- Queue has **ordered** according to the **priority** data item
  - along with combo of **top / pop** method calls reads the items in priority order and removes from queue
- Items of **same priority** placed in the **respective order** they **entered**. 

## Using iterators

- Indicated that **containers** give *access* to **items** via **iterators**
  - The implication is that **iterators** are just **pointers**
    - This is **deliberate** as *iterators* behave **like pointers**
      - They are often **objects** of **iterator classes** `<iterator>` header.

- All iterators **have these behaviour.**

| **Operator** | Behaviours                                                   |
| ------------ | ------------------------------------------------------------ |
| *            | Gives access to the **element** at the **current position**  |
| ++           | Moves **forward** to the **next element** (usually you will use the **prefix operator**)(this is only if the **iterator allows forward movement**) |
| --           | Moves **backward** to the **previous element** (usually you will use the prefix operator)(this is only if the iterator allows backward movement) |
| == and !=    | **Compares** if **two iterators** are in the same position   |
| =            | **Assigns an iterator**                                      |

- Unlike **c++ pointer**, which assumes the **data is contiguous**
  - Iterators used for **more complex data structures**
    - Such as **linked list** $\to$ not contiguous necessarily. 
      - operators `++` and `--` work **as expected** regardless of **underlying storage mechanism**
- `<iterator>` header declares the `next` global function that will *increment an iterator* 
- `advance` function also that **changes** iterator by a **specified** number of positions
  - forward / backward depending on whether the parameter is negative and the direction allowed by the iterator
- `prev` to **decrement** an iterator by **one or more positions**
- `distance` determine **how many items** are **between two iterators**

---

- All containers > have `begin` method
  - Returns **iterator** to the **first item**
- An `end` method
  - Returns iterator to **after the last item**
- With these you can **iterate** over every item by *calling* `begin` and then **incrementing** until it has the value *returned* from `end`.

- The * operator $\to$ gives **access** to the element in the **container**
  - If the iterator is **read / write** $\to$ means the **item can** be **changed** 

---

- Containers have `cbegin` and `cend` methods
  - Returns a **constant iterator** to give **read only** access to the **elements**

```c++
    vector<int> primes { 1,2,3,5,7,11,13 }; 
    const auto it = primes.begin(); // const has no effect 
    *it = 42; 
    auto cit = primes.cbegin(); 
    *cit = 1;                       // will not compile
```

- `const` has **no effect** as the variable is `auto` 
  - The **type** is already **deduced** from the **item** used to **initialize** the **variable**
  - https://stackoverflow.com/questions/5070521/does-const-auto-have-any-meaning

- `cbegin` method defined to **return** a **const iterator**
  - Therefore you can **not** *alter* the **item** it **refers to**

---

- The `begin` and `cbegin` methods $\to$ return **==forward iterators==**
  - Therefore the `++` operators moves them **forward**

- Containers may **support** **==reverse iterators==** where `rbegin` is the **last item** in the **container**
  - Item **before** the **position** returned by **end**
- `rend` $\to$ position **before** the **first item**
  - There is also
    - `crbegin`
    - `crend`
  - const reverse iterators.

- `++` moves **backwards** for some **reverse iterator**.

```c++
    vector<int> primes { 1,2,3,5,7,11,13 }; 
    auto it = primes.rbegin(); 
    while (it != primes.rend()) 
    { 
        cout << *it++ << " "; 
    } 
    cout << endl; // prints 13,11,7,5,4,3,2,1
```

- `++` operator $\to$ will **increment** the **iterator** according to the **type** of the **iterator** that its applied to.
  - Vital to **note** that the $!=$ operator used here **to determine** if **looping should end**
    - This is **defined** on **all iterators** this operator.

---

- The **iterator type** is **ignored** using the **auto keyword**
  - All containers have **typedef** for all of the **iterator types** they use
- Previous case , use this:

```c++
    vector<int> primes { 1,2,3,5,7,11,13 }; 
    vector<int>::iterator it = primes.begin();
```

- Containers that **allow forward iteration**
  - Have a `typedef` for `iterator` and `const_iterator` 
- Containers that allow **reverse iteration**
  - Have `typedef` for `reverse_iterator` and `const_reverse_iterator`.

---

- Containers also have `typedef` for `pointer / const_pointer` 
  - For methods that **return** the **pointers** to the **elements**
  - `reference` and `const_reference` for methods that **return references** to **elements**.

- These **type definitions** allow you to write **==generic code==** where you **do not know** the **type** in some **container**
  - The code can still **declare** variables of **correct type.**

https://stackoverflow.com/questions/29944985/is-there-a-way-to-pass-auto-as-an-argument-in-c#:~:text=C%2B%2B20%20allows%20auto%20as%20function%20parameter%20type&text=As%20an%20abbreviated%20function%20template.&text=A%20placeholder%2Dtype%2Dspecifier%20designates,by%20deduction%20from%20an%20initializer.

- Iterators often **implemented** by **classes**

  - These types may **only allow iteration** in **one direction** 
  - Or **both directions** thus implement the `++` and `--` rather than one each.

- Iterations of 

  `list` , `set`, `multiset`, `map` and `multimap` classes are **bidirectional**

- `vector`, `dequeue`, `array` and `string` have iterator that allow **random access**

  - Therefore these **iterator types** have **same behaviour** as **bidirectional iterators**
    - Also have **pointers** like **arithmetic** therefore can be **changed** more than **one item** position at a time.

## Input and output operators

- **==input iterator==** only **move forward** and have **read access** 

- An **==output iterator==** only **moves forward** but has **write access**

- They have **no random access** 

- They **do not allow backward movement**

- An *output stream* can be used with an **output iterator**

  - Assign the **dereferenced** iterator with a **data item** in **order** to *write* that **data item** to the **stream**.

- An **input stream** $\to$ could have an **input iterator**
  - Then **dereference** the **input** iterator to obtain access to the **next item** in the **stream**
    - For some **output operator** $\to$ only *valid use* of the **dereference operator** ***** is on **LHS** of assignment.
  - Make no sense to check **the value** of an **iterator** with `!=` 
    - You can **not check** if *assigning a value* via the **output iterator** is actually **successful**.

---

- The `transform` function takes **three iterators** and a **function**
  - The *first* **two iterators** are **input iterators**
    - Indicate a **range of items** to **be transformed** by the **function**

- **Result** place **in a range of items** (*same size* as the **range** of **input iterator**)
  - The *first* of **which** is **indicate** by the **third iterator**, an **==output iterator==**.
    - One way:

```c++
    vector<int> data { 1,2,3,4,5 }; 
    vector<int> results; 
    results.resize(data.size()); 

	/*
		begin/end methods return iterators on the data cntainer that are safe to use an input iterators
		
		begin method on the results container only used as an output iterator as long as the container has enough
		allocated items
		
		for this case > this is true as they have been allocated with the size of data
		
		function transforms each input by passing it to the lambda given in the last parameter
		
		third parameter is an output iterator, means you expect the function to write values via the iterator
	*/
    transform( 
       data.begin(), data.end(),  
       results.begin(), 
       [](int x){ return x*x; } );
```

- Works $\to$ requires **extra  step** to **allocate** the **space**
  - Have **extra allocations** of *default objects* in the **container** just so you can **overwrite**

> The **output iterator** does **not have to be another container**
>
> - Can be the **same container.**

```c++
    vector<int> vec{ 1,2,3,4,5 }; 
    vec.resize(vec.size() * 2); // resize by double the size

	//starting from first item to the 5th item, the output iterator starts at index at the 6th item
    transform(vec.begin(), vec.begin() + 5, 
       vec.begin() + 5, [](int i) { return i*i; });

	// this makes vec become {1,2,3,4,5,1,4,9,16,25}
```

- Another form of **output iterator** is the *inserter*
  -  The `back_inserter` used on **containers** with `push_back` and 
  - `front_inserter` > `push_front` 
- An **inserter** calls the `insert method` 
  - Example use the `back_inserter` as such

```c++
    vector<int> data { 1,2,3,4,5 }; 
    vector<int> results; 
    transform( 
       data.begin(), data.end(),  
       back_inserter(results), 
       [](int x){ return x*x; } ); // 1,4,9,16,25
```

- Result of the **transformation** are *inserted* to the **results container**
  - Uses the *temp object* created from `back_inserter` 
- Using a `back_inserter` ensures when the **transform** function **writes** via the iterator $\to$ item is **inserted** into the **wrapper container** using `push_back`. 
  - Note that **the results container** should be **different** to the **source container**

---

- Obtain values **in reverse order**
  - If container supports it $\to$ `push_front` method such as `deque` 
    - Use `front_inserter` 
- The `vector` class does **not have** the `push_front` method
  - Does have **reverse iterators** so you **use these instead** to obtain **results**

```c++
    vector<int> data { 1,2,3,4,5 }; 
    vector<int> results; 
    transform( 
       data.rbegin(), data.rend(),  
       back_inserter(results), 
       [](int x){ return x*x; } ); // 25,16,9,4,1
```

- All you need to do to **reverse results** to change `begin` > `rbegin` , `end` > `rend` 

## Stream iterators

- Adapter classes in `<iterators>` 
  - Used to **read items** from an **input stream** or **write** to **output**

- Example $\to$ we used **iterators** via *ranged for loops* to **print out** the **contents** of a **container**:

```c++
    vector<int> data { 1,2,3,4,5 }; 
    for (int i : data) cout << i << " "; 
    cout << endl;
```

- Create an **output stream** iterated based on **cout**
  - So the **int** values are written to the `cout` stream via the **iterator** using the **stream operator** `<<` 
    - To **print** a *container* of `int` values $\to$ just **copy** the **container** to the **output iterator**

```c++
    vector<int> data { 1,2,3,4,5 }; 
/*
	param 1 > the output stream it adapts
	optional param 2 > delimiter string used between each item
*/
    ostream_iterator<int> my_out(cout, " "); 
/*
	copy function from <algorithm> copy the items in specified range to the output iterator in last param.
*/
    copy(data.cbegin(), data.cend(), my_out); 
    cout << endl;
```

- There is an `istream_iterator` that will **wrap** an **input stream** *object* > provides an **input iterator**
  - Uses `>>` operator to **extract objects** of the *specified type*
    - Read via **stream iterator**.
- Reading data from **stream** more **complex** than **writing**
  - Must be **detection** of when there is **no more data** in  the **input stream** for the *iterator* to **read** (*EOF*)

---

- `istream_iterator` class has **two constructors**
  1. A **single parameter** one that is the **input stream** to *read*
  2. **default** , creates the **end of stream iterator**
- **End of stream iterator** $\to$ indicates when there is **no more data** in the stream.

```c++
    vector<int> data; 
/*
	first call to copy, provides two input iterators
	1. first iterator , 2. last parameter
	
	last parameter created from back_inserter, means the items are inserted into the vector object
	- essentially directly giving access from the  user to add to data.
	
	input iterators based on cin input stream, therefore the copy function reads int values from console (each seperate by white space, until there is non left > press ctrl+z example to end stream or type non numeric value
*/
    copy( 
       istream_iterator<int>(cin), istream_iterator<int>(), 
       back_inserter(data)); 

    ostream_iterator<int> my_out(cout, " "); 
    copy(data.cbegin(), data.cend(), my_out); 
    cout << endl;
```

- As you can **initialize** a container with a **range** of **values** given by **iterators**
  - Use the `stream_iterator` as **constructor parameters**

```c++
    vector<int> data {istream_iterator<int>(cin), istream_iterator<int>() };
```

- The **constructor** called using the **initializer list syntax**
  - If you use **parentheses**, compiler will interpret as **declaration** of a **function**

---

- Noted earlier $\to$ the `istream_iterator` use the **stream's** `>>` operator to **read objects** of the **specified type**
  - From the **stream** and this **operator** uses *whitespace* to **delimit items** (thus just ignores it)
    - If you **read in a container** of *string objects*
      - Then **every** word you **type on the console** is an **item in the container**
- A `string` is a **container** of **characters**
  - Can be **init** using **iterators**
    - Try to **input data** into a `string` from the **console** via `istream_iterator` 

```c++
    string data { istream_iterator<char>(cin), istream_iterator<char>() };
```

- For **this case** $\to$ the *stream* is `cin` 
  - Could be an `ifstream` object to a **file**
- Problem is that `cin` object will **strip** out the **white space**
  - Therefore the `string` object contain **everything** that you enter **except for white space**, thus **no space / newline**

---

- Caused by the `istream_iterator` using the **stream** `>> operator` 
  - avoid using the `istreambuf_iterator` :

```c++
    string data { istreambuf_iterator<char>(cin), istreambuf_iterator<char>() };
```

- This reads **each character** from the **stream** then **copies** one into the **container**
  - With **no processing** of `>>` 

## Using iterators with standard c library.

- Often requires **pointers to data** working with c library.
- C requiring  a string takes a `const char *` pointer to **the character array** containing the **string**
- C++ adapts this library in c standard library to work with there classes
- For `string objects` $\to$ simple solution
  - When you need a `const char*` pointer $\to$ call `c_str` method on `string` object.

---

- Containers that **store data contiguously** (*array / string /data*)
  - Have `data` method gives access to the **container's data** as a `C` array.
- Have `[]` operator access to **their data**
  - Treat **address** of the **first item** as being `&container[0]` $\to$ where `container` is the **container object**.
- If the **container** is *empty* $\to$ address **invalid**
  - Call **empty method** before **using therefore**
- Use`size` method to return **number of items** in a container
  - For any C function $\to$ that takes a **pointer** to the start of a **C array** and **its size**
    - Call with `&container[0]` and value from `size` method.

---

- Using `begin` method wont work as it **returns** the *iterator object* 
  - To obtain a **C pointer** $\to$ first item
    - call `&*begin` $\to$ **dereference** the **iterator** that is *returned* via the `begin ` method.

- `&container[0]` more readable.

---

- If **does not store contiguous** (`deque` and `list`)
  - Obtain a **C pointer** via *copying* data to **temporary vector**
- https://www.geeksforgeeks.org/initialize-a-vector-in-cpp-different-ways/

```c++
    list<int> data; 
    // do some calculations and fill the list 
    vector<int> temp(data.begin(), data.end()); 
    size_t size = temp.size(); // can pass size to a C function 
    int *p = &temp[0];         // can pass p to a C function
```

- For this case $\to$ chosen to use a **list** and the **routine** will **manipulate** the *data object*
  - Later $\to$ these values will be **passed** to a `C` function so the **list** is used to **initialize** a *vector object*
    - These values are **obtained** from ` vector`.

# Algorithms

- Generic use as in the type doesnt matter for which you apply them just using iterators instead.

## Item iteration

- Often algorithms in `<algorithm>` take **ranges** then **iterate** over those with some **action**.

---

### fill

- Fills a container with **some value**
- Takes **2 iterators** of range that is **placed** into each **position of the container**

```c++
    vector<int> vec; 
    vec.resize(5); 
    fill(vec.begin(), vec.end(), 42);
```

- Must **pass iterators** for the **range**
  - Must *already have values* therefore use `resize` 
- Alternative version `fill_n` 
  - Specify range by **single character** *iterator* to the **start** of the **range** and a **count** of **items** in **the range**

---

### Generate

- Similar to fill $\to$ for a **single value** $\to$ has a **function**
  - Either **function** / **functor** or **lambda**
- Called to **provide** each item in the container
  - Therefore has **no parameters** and **returns an object** of the **type accessed** by the **iterator**.

```c++
   vector<int> vec(5); 
    generate(vec.begin(), vec.end(),  
        []() {static int i; return ++i; });
```

- Make sure `generate` function is **passed a range** that *already exists*
  - This code does via passing **initial size** as a **constructor parameter**
- The *lambda* here has a `static`  variable
  - This is **incremented** on each call
    - Therefore after **generate** function is **over** 
      - vector contains `{1,2,3,4,5}`
- Alternative $\to$ `generate_n` 
  - Specifies the **range** via a **single iterator** to the **start** of a **range** AND a **count** of the **items** in the *range*.

### for_each

- Iterates over **all items** specified via the **iterators** range
  - Then **dereferences** the **iterator** and **passes** this to **the function**
- Print the **contents** of the container using this below

```c++
    vector<int> vec { 1,2,3,4,5 }; 
    for_each(vec.begin(), vec.end(),  
         [](int& i) { i *= i; });
```

- After **calling** $\to$ items in `vector` replace with the **square of these items**
  - If you **use** a `functor ` or a `lambda` expression $\to$ can pass a **container** to *capture said result*

```c++
    vector<int> vec { 1,2,3,4,5 }; 
    vector<int> results; 
    for_each(vec.begin(), vec.end(),  
         [&results](int i) { results.push_back(i*i); });
```

- **Container** declared to **accept** the **results** of *each call* to the **lambda expression**
  - Along with the **variable** is *passed* by **reference** to the **expression** via **capturing it**.

> Recall from *[Chapter 3](https://learning.oreilly.com/library/view/modern-c-efficient/9781789951738/ddcfe6af-f0bb-4c28-ad97-9c402bc43799.xhtml),* Using Functions, that the square brackets contain the names of the captured variables declared outside the expression. Once captured, it means that the expression is able to access the object.

- The **result** of every iteration is passing the squares into the results vector.

---

### Transform

- Has **two forms** $\to$ both **provide** a **function** (*a pointer* , *functor*, lambda) $\to$ both have **input range** of **items** in a container passed **via iterators**

- Similar to `for_each` 
- `transform` function **allows** you to **pass an iterator** to a *container* that is **used** to **store results**
  - Function must **have a single parameter** that is the **same type / reference** of the **type rereferred** to the **input iterators**
    - must **return the type** accessed by **output iterator**.

- Other version of **transform** uses a **function** to *combine values in two ranges*
  - Therefore has **two parameters** (which are the **corresponding** items in the **two iterators**)
    - Then **return** the **type of output iterator**
    - Only need to **give full range** of items **in one input** ranges as they are assumed to be **at least as large**
      - Therefore **beginning iterator** of the **second range** is required only

```c++
    vector<int> vec1 { 1,2,3,4,5 }; 
    vector<int> vec2 { 5,4,3,2,1 }; 
    vector<int> results; 
    transform(vec1.begin(), vec1.end(), vec2.begin(), 
       back_inserter(results), [](int i, int j) { return i*j; });
```

## Getting information

- After the **values** in a **container**
  - Can **call functions** to obtain **information** about *those items*

### Count

- Used to **count** the **number of items** with a **specified value** in a **range.**

```c++
    vector<int> planck{ 6,6,2,6,0,7,0,0,4,0 }; 
    auto number = count(planck.begin(), planck.end(), 6)
```

- returns 3 as there  6 threes.
- return type is the **type specified** in the `difference_typetypedef` of the container https://stackoverflow.com/questions/38540198/stl-containers-difference-type-typedef
  - This case its **int**  

- `count_if` is the same but **passes** a *predicate* that takes a **single parameter**
  - The *current item* in the **container**, return bool if it was counted or not.

- `count` counts **occurrences**
  - To **aggregate** all the values
    - Use `accumulate` function `numeric` 

https://www.geeksforgeeks.org/accumulate-and-partial_sum-in-c-stl-numeric-header/

- This **iterates** over the **range** , access each item and **keep running sum of all items** 

- Sum carried out via  `+` operator of the type
  - A version that takes a **binary function** $\to$ 2 parameters of the **container type** and **return** the **same type**
    - Specifies **what happens** when you *add two such type together*https://www.cplusplus.com/reference/functional/binary_function/

---

- The `all_of` , `any_of` and `none_of` 
  - Passed a **predicate** of **single arg** / **same type** to container
- all of true only if predicate true for all
- any of requires at least one
- none of requires none. 

## Comparing containers

- Two containers of data
  - There are **various ways** to **compare**
- using standard <, <=, ==, !=, > and >=
- == and != compare containers , both in terms of **how many items** and the **value** of those items.
  - If they **differ in number of items** / **different values** / **both** they are **not equal**.
- Other ones prefer **value** over *number of items*

```c++
    vector<int> v1 { 1,2,3,4 }; 
    vector<int> v2 { 1,2 }; 
    vector<int> v3 { 5,6,7 }; 
    cout << boolalpha; 
    cout << (v1 > v2) << endl; // true , has more values in it
    cout << (v1 > v3) << endl; // false, has less values but of higher value.
```

- Compare ranges with `equal` function
  - Passed **2 ranges** $\to$ assumed to be **same size**
  - Therefore **only an iterator** to the **start** of the **second range** is required.
- Compares **corresponding** items in **both ranges** via `==` operator for the **type accessed** by the **iterator**
  - Or **user supplied boolean predicate**
- Requires **every comparison** to come out as *true*.

https://www.geeksforgeeks.org/stdequal-in-cpp/

### mismatch

- `mismatch` function compares **corresponding** items in **two ranges**
  - This function returns a **pair** object with **iterators** in each of the **two ranges** for *the first item* that is **not the same**
- Can provide a `comparison function`

### is_permutation

- `is_permutation` is similar in that **it compares** the **values** within **two ranges**
  - Returns `true` if the **two ranges** have the **same values** but *not in the same order*

https://www.geeksforgeeks.org/stdmismatch-examples-c/

## Changing items

- `reverse` function acts on a **range** within a **container**

  - Then **reverses** the *order* of the items.
    - Therefore the **iterators** must be **writeable**

- `copy` and `copy_n` will **copy every item** from a **range **to **another** in a *forward* direction

  - `copy` $\to$ the **input range** dictated via **two input iterators**

  - `copy_n` $\to$ range is an **input iterator** and **count of items**

- `copy_backward` function copy items **starting** at the **end of the range** https://www.javatpoint.com/cpp-algorithm-copy_backward-function
  - Therefore have **items** of **same order** to **original**
    - Means the **output iterator** indicates the **end** of the **range** to **copy to**
      - Can also **copy items** if they *satisfy* some conditions specified via **predicate**.

---

#### reverse_copy

- create a copy in the **reverse order** to the **input range**
  - The function **iterates backwards** through the **original** and **copies** items to the **output range** forward

#### move , move_backward

- *semantically equivalent* to *copy* and copy_backward
  - Therefore in following **original container** has the **same values** after **operation**

```c++
        vector<int> planck{ 6,6,2,6,0,7,0,0,4,0 }; 
        vector<int> result(4);          // we want 4 items 
        auto it1 = planck.begin();      // get the first position 
        it1 += 2;                       // move forward 2 places 
        auto it2 = it1 + 4;             // move 4 items 
        move(it1, it2, result.begin()); // {2,6,0,7}
```

- Copy 4 items in the first container to the second starting at the third position

#### remove_copy, remove_copy_if 

- Iterate through the **source range** and **copy items** other than them **with a specified value**

```c++
        vector<int> planck{ 6,6,2,6,0,7,0,0,4,0 }; 
        vector<int> result; 
        remove_copy(planck.begin(), planck.end(),  
            back_inserter(result), 6);
```

- `planck` left the same as before and the result contains `{2,0,7,0,0,4,0}`
  - if variant is similar but takes a **predicate** rather than **integer**.

#### remove, remove_if

- Act upon a **single range**
  - Iterate to find a **specific value** / pass item to **predicate**
    - item removed $\to$ items **later** in the **container** are *shifted forward*
    - Container **remains same size** $\to$ items **remain** as they were.

- Purpose
  - Only aware of **reading / writing** items via **iterators** (*generic* upon **all containers**)
    - To **erase** some item $\to$ function needs `erase` method of the container
    - `remove` functions only have access to iterators.

- To **remove** the **items** at the end
  - Must `resize` accordingly
    - Typically $\to$ means calling some **suitable** `erase` method on the container
      - made possible via the **remove** method returning an **iterator** to the **new end position**

```c++
        vector<int> planck { 6,6,2,6,0,7,0,0,4,0 }; 
        auto new_end = remove(planck.begin(), planck.end(), 6); 
                                             // {2,0,7,0,0,4,0,0,4,0} 
        planck.erase(new_end, planck.end()); // {2,0,7,0,0,4,0}
```

#### replace, replace_if

- *iterate* over a **single range** 
  - If the **value** is a **specified value** such as `replace` 
  - or returns **true** via **predicate** 
    - The item is **replaced** with some **specified value**

#### replace_copy, replace_copy_if

- leave the **original alone** and make **changes** to some **other range**
  - like `remove_copy` and `remove_copy_if` 

#### rotate

https://www.geeksforgeeks.org/rotate-in-cpp-stl/

- Treat the **range** as if the **end** is **joined** to the **start**
  - Therefore you can **shift items forward** so when some item **falls** off **end**
    - Placed in **first position**
      - To move every item **forward 4** $\to$ 

```c++
        vector<int> planck{ 6,6,2,6,0,7,0,0,4,0 }; 
        auto it = planck.begin(); 
        it += 4; 
        rotate(planck.begin(), it, planck.end());
```

- result of this is `{0,7,0,0,4,0,6,6,2,6}`

#### rotate_copy

- Does the same thing, but rather than affecting the original, copies the items to **another container**

#### unique

- Acts on a **range** and **removes** (manner explained before)
  - The **items** that are **duplicates** of **adjacent items** $\to$ can provide  **a predicate** to test of equality.
- Function only checks **adjacent items**
  - Therefore a **duplicate** later in the container can **remain**
    - To remove **all duplicates**
      - Must **sort container first** 

#### unique_copy

- copy **items** from **one range** to **another** only if they are **unique**
  - Thus one way to **remove duplicates** $\to$ use this function on a **temporary container** and *then assign* the original to the **temporary**

```c++
        vector<int> planck{ 6,6,2,6,0,7,0,0,4,0 }; 
        vector<int> temp; 
        unique_copy(planck.begin(), planck.end(), back_inserter(temp)); 
        planck.assign(temp.begin(), temp.end());
```

- output of plank after `{6,2,6,0,7,0,4,0}`

#### iter_swap

https://stackoverflow.com/questions/14024228/iter-swap-versus-swap-whats-the-difference

- Swap items indicated by **two iterators**

#### swap_ranges

-  swaps item in **one range** to the **other range**
  - Second range $\to$ indicated by a **single iterator** 
    - Assumed to **refer ** to **a range** of the **same size** as **first**

## Finding items

#### min_element, max_element

- return **iterator** to the **smallest/maximum item** in range
- Functions are **passed iterators** for the **range** of items to check
  - Alongside **optional predicator** , default as < 

```c++
        vector<int> planck{ 6,6,2,6,0,7,0,0,4,0 }; 
        auto imin = min_element(planck.begin(), planck.end()); 
        auto imax = max_element(planck.begin(), planck.end()); 
        cout << "values between " << *imin << " and "<< *imax << endl;
```

- `imin / imax` are **iterators**
- Obtain both as a `pair` with `minmax_element` 

#### adjacent_find

- `adjacent_find` return **position** of the **first two items** that have the **same value**
  - Provide a **predicate** to **determine** what *same value means*
    - Allows you to **search for duplicates** and obtain the **position** of *those duplicates*

```c++
        vector<int> vec{0,1,2,3,4,4,5,6,7,7,7,8,9}; 
        vector<int>::iterator it = vec.begin(); 

        do 
        { 
            it = adjacent_find(it, vec.end()); 
            if (it != vec.end()) 
            {  
                cout << "duplicate " << *it << endl; 
                ++it; 
            } 
        } while (it != vec.end());
```

- Code has **sequence of numbers** where there are **some numbers** that are **duplicated** being *next to one another*
  - For this case $\to$ there are **three adjacent duplicates**, 4 by 4,  7 by 7 by 7
- **do loop** calls *adjacent_find* repeatedly until it **returns** the *end iterator*
  - Indicating it has **searched all items**
    - When **duplicate found** $\to$ code **prints** out the value , then **increments** the **start position** for **next search**

---

##### Find

- Search a container for a single value
- Return iterator to that item or the end iterator if the value cannot be found.
- `find_if` variant passed a **predicate** $\to$ returns iterator to the **first item** it finds that satisfies **this predicate**
- `find_if_not` find the **first item** that *does not satisfy* the **predicate**.

---

- Many functions that are **given two ranges**
  - One is the **range** to *search* $\to$ the **other** has the **values** to *look for*
  - Different functions $\to$ either look for **one / all** items in search criteria.
    - Use the `==` operator for the **type** that the **container** holds the **predicate**

---

- `find_first_of` 
  - Returns the **position** of the first item it **finds** within the **search list**

##### search

http://www.cplusplus.com/reference/algorithm/search_n/

- Looks for a **specific sequence** , then returns the **first position** of the **whole sequence.**
-  `search_n` function looks for a **sequence** that is a **value repeated** a **number of times**
  - (the value and the **repeat** are **given**)

- in a specified container range.

## Sorting items

- ​	SORT > SEARCH 

##### Sort

- order the items in a **range** according to **<** operator on a **predicate** that you **provide.**
- Equal items , order after sort is **not guaranteed**
  - Call `stable_sort` if its an **important order**
  - https://www.cplusplus.com/reference/algorithm/stable_sort/

- Preserve the **input range** and **copy** the **sorted items** $\to$ another range $\to$ `partial_sort_copy` 
- https://stackoverflow.com/questions/20364848/is-partial-sort-copy-the-fastest-c-partial-sort
- https://www.geeksforgeeks.org/stdpartial_sort_copy-in-cpp/#
- Make sure the output range is of **suitable size**

---

##### is_sorted

- Check if some range is sorted
  - Iterate through all items and return **false** if it finds an item not in the correct sorted order
    - This case $\to$ locate the **first** item that is **out of sort order** by *calling* the `is_sorted_until` function.

---

##### Partial sort

- Does **not place** every item in the **exact order** relative to every other item
  - Creates **two groups** / **partitions**
    1. First partition $\to$ has the **smallest items** (*not necessarily* in any order)
    2. Other $\to$ has the **biggest items**

- Guaranteed that the **smallest items** are in the **first partition**
  - Requires **three iterators**
    1. Two of which are the sort range
    2. Somewhere **between** the **other two** that indicates the **boundary** before which are the **smallest values**

```c++
    vector<int> vec{45,23,67,6,29,44,90,3,64,18}; 
    auto middle = vec.begin() + 5; 
    partial_sort(vec.begin(), middle, vec.end()); 
    cout << "smallest items" << endl; 
    for_each(vec.begin(), middle, [](int i) {cout << i << " "; }); 
    cout << endl; // 3 6 18 23 29 
    cout << "biggest items" << endl; 
    for_each(middle, vec.end(), [](int i) {cout << i << " "; }); 
    cout << endl; // 67 90 45 64 44
```

- sorts five smallest and five larger into seperate containers.

----

##### nth_element

- Sorts like partial sort
  - Provide **iterator** to teh **nth element** and it ensures the *first n* items in the **range are the smallest**
- The `nth_element` function is **faster** than `partial_sort` 
  - You are **guaranteed** that the items before the `nth` element are **less** than or **equal** to the `nth element`
    - There are **no other guarantees** of the *sort order* within the **partitions**.

- `partial_sort` and `nth_element` are *versions* of the **partitioned sort functions**
  - The `partition` function is a **more generic version**
    - Pass the **function** some *range* and a *predicate* that **determines** in which of the **two partitions** an item is *placed*
  - Items that **meet predicate** $\to$ placed in **first partition** of the range, other items are in the **other partition following**
  - First item of the second $\to$ called the **partition point** and is *return value* of this function
    - Can calculate later by **passing iterators** to the **partitioned range** and the **predicate** to the *partition_point* function
- `partition_copy` 
  - Has partition values
  - Leaves **original range intact** $\to$ places in a range **already allocated**
- These partition functions do **not guarantee** the *order of equivalent items*
  - Call `stable_partitian` if you require this
- Determine if some container is **partitioned** via `is_partitioned` function

---

##### Shuffle

- This **rearranges** the items in a **container** to **random order**
  - Requires a **uniform random number generator** from `<random>` library
    - Example $\to$ following **fills** some container with **ten integers** , then places them in **random order**

```c++
vector <int> vec;
for (int i = 0; i < 10; i++) vec.push_back(i);
random_device rd;
shuffle(vec.begin(), vec.end(), rd);
```

##### Heap

- A **heap** is some **partially sorted** sequence in which the **first item** is *always the largest*
  - items are **added** and **removed** from the **heap** in **O(log n)** 
- Heaps based on **sequence containers** 
  - Rather than **standard library** giving a **container class**
    - have to use **function calls** on some **existing container**
- Create a *heap* from some **existing container.**
  - pass the **range iterators** to the `make_heap` function
    - This orders the **container** as a **heap**.

- Add **new items** using `push_back` method
  - Each time you do this $\to$ call `push_heap` to **re-order** the **heap**
- Get item from heap $\to$ `front` on container and them **remove** with `pop_head` 
  - Ensure the **heap** stays ordered
    - Test if a **container** is arranged as a heap via `is_heap` 
    - If **not entirely** arranged as heap $\to$ obtain **iterator** to the **first item** that *does not satisfy* the **heap criteria** by calling `is_heap_until` 
- Sort a heap to **sorted sequence** via `sort_heap` 

---

- With a **sorted container**:

##### Bounds

- `lower_bound` / `upper_bound` 
  - lower $\to$ return position of **first element** that has a **value** 
  - upper $\to$ return position of **next item** that is *greater*  than the **value provided**

- `includes` $\to$ tests to check if **one sorted range** contains **items** in a **second sorted range**.

---

##### set_

- Functions starting with `set_` combine **two sorted sequence** to some **third container**
- `set_difference` $\to$ **copy** the **items** that are in the **first sequence** , but **not** in the **second sequence**
  - This is *not a symmetric action* as it **does not include** the items in the **second sequence** (just ones that are unique to first respect to second)
- `set_symettric_difference` function obtains this property
- `set_intersection` **copy** the items in **both sequence** 
- `set_union` will **combine** the **two sequences**
- `merge` $\to$ combine the **two sequences** also
  - Difference between these two is that for `set_union` , if an item is **in both** of the sequences $\to$ a **single copy** placed in results
  - For merge $\to$ there are **two copies**

---

##### equal_range

- used on **sorted ranges**.

- use `equal_range` to obtain **range of elements** that are **equivalent** to a **value passed** or **predicate**

- Returns a **pair of iterators** that *represent* this range.

---

##### binary_search

- Test if a value is **in the container**
  - Passed **iterators** indicating the **range** to  *test*
  - Passed a value , returning true if there is an **item** in the **range equal** to that value
    - Provide **predicate also**

## Using Numeric Libraries

- Various libraries for **numeric manipulations**

### Compile Time Arithmetic

- Fractions cause error with **precision** as there may **be insufficient** number of **significant figures**.
  - Therefore losing accuracy with **further arithmetic**.
- Decimal $\to$ binary , bound to **loss accuracy**

#### Ratio

- This *<ratio>* library $\to$ gives classes that **allow** you to **represent fractional numbers** as *objects*
  -  That are **ratios of integers**
    - Perform **fraction calculations** as *ratios*
- Only after all **fractional arithmetic**
  - You can **convert** the *number to decimal*
    - This means that the **loss of accuracy** is **minimized**
- Calculations performed by classes of `ratio` library are **carried** out at **compile time**
  - Therefore the compiler can **catch errors** such as **division by zero / overflows**

----

- Usage:

```python
    ratio<15, 20> ratio; 
    cout << ratio.num << "/" << ratio.den << endl;
```

- Fractional arithmetic out via **templates**
  - These are **specializations** of the `ratio` template
- First sight $\to$ may appear **little odd**
  - GET USED TO IT AHHH, NOTHING MAKES ME TRULY HAPPY.

```python
    ratio_add<ratio<27, 11>, ratio<5, 17>> ratio; 
    cout << ratio.num << "/" << ratio.den << endl;
```

- Prints out **514/187**
  - Data members are **static** $\to$ no sense to **create variables**
- Arithmetic **carried** out using **types** rather than **variables**
  - best to access members via those types

```python
    typedef ratio_add<ratio<27, 11>, ratio<5, 17>> sum; 
    cout << sum::num << "/" << sum::den << endl;
```

- Use the **sum type** as **parameter** to any of the **other operations** you can **perform**
- Four **binary arithmetic operations**
  1. `ratio_add` 
  2. `ratio_subtract` 
  3. `ratio_multiply` 
  4. `ratio_divide` 

- Comparisons carried out via:
  1. `ratio_equal` 
  2. `ratio_not_equal` 
  3. `ratio_greater` 
  4. `ratio_greater_equal` 
  5. `ratio_less` 
  6. `ratio_less_equal` 

```python
   bool result = ratio_greater<sum, ratio<25, 19> >::value; 
   cout << boolalpha << result << endl;
```

- Operation tests to check if the **calculation** performed *before* 514/187 is **greater** than the **fraction 25/19**
- Compiler picks up **divide by zero errors** and **overflows**, thus following wont **compile**

```python
    typedef ratio<1, 0> invalid; 
    cout << invalid::num << "/" << invalid::den << endl;
```

- Vital to **point** out that the **compiler** will **issue the error** on the **second line**
  - When the **denominator** is *accessed* 
- There are `typedefs` of **ratio** for **SI** prefixes
  - Therefore you can **perform calculations** in **nanometers**
    - When you need to **present data** within **meters**  use `nano` type to **obtain** the *ratio*

```c++
    double radius_nm = 10.0; 
    double volume_nm = pow(radius_nm, 3) * 3.1415 * 4.0 / 3.0; 
    cout << "for " << radius_nm << "nm " 
        "the volume is " << volume_nm << "nm3" << endl; 
    double factor = ((double)nano::num / nano::den); 
    double vol_factor = pow(factor, 3); 
    cout << "for " << radius_nm * factor << "m " 
        "the volume is " << volume_nm * vol_factor << "m3" << endl;
```

- This is a calculation on spheres in **nanometeres (nm)**
  - Sphere has **radius** of 10nm
- Therefore the **first calculation** gives the **volume** as `4188.67` **nm3**
- Second calculation convers **nanometres** to **meters**
  - Factor determined from the **nano ratio** (not volumes the **factor is cubed**)
- Can define a class to **perform such conversions**

```c++
template<typename units> 
    class dist_units 
    { 
        double data; 
        public: 
            dist_units(double d) : data(d) {} 

        template <class other> 
        dist_units(const dist_units<other>& len) : data(len.value() *  
        ratio_divide<units, other>::type::den / 
        ratio_divide<units, other>::type::num) {} 

        double value() const { return data; } 
    };
```

- Class defined for a **particular type** of **unit**
  - Expressed via **instantiation** of the `<ratio>` template
    - Class has a **constructor** to **init** for values in **those units** and a **constructor** to convert from **other units** 
      - This just **divides** the **current units** by the **units** of the *other type*
- Used as such:

```c++
    dist_units<kilo> earth_diameter_km(12742); 
    cout << earth_diameter_km.value() << "km" << endl; 
    dist_units<ratio<1>> in_meters(earth_diameter_km); 
    cout << in_meters.value()<< "m" << endl; 
    dist_units<ratio<1609344, 1000>> in_miles(earth_diameter_km); 
    cout << in_miles.value()<< "miles" << endl;
```

- First variable based on **kilo**
  - Therefore the **units** are *kilometres*
    - To convert **this to meters** $\to$ second variable type based on `ratio<1>` 
      - Same as the `ratio<1,1>` 

- Values in `earth_diameter_km` are mult by 1000 when placed `in_meters` 
  - The **conversion** miles is more involved

---

- There are 1609.344 m in a mile. 
- The ratio used for the in_miles variable is 1609344/1000 or 1609.344. 
- We are initializing the variable with the earth_diameter_km, so isn't that value too big by a factor of 1000?
-  No, the reason is that the type of earth_diameter_km is dist_units<kilo>, so the conversion between km and miles will include that factor of 1000.

### Complex Numbers

```c++
complex<double> a(1.0, 1.0); 
    complex<double> b(-0.5, 0.5); 
    complex<double> c = a + b; 
    cout << a << " + " << b << " = " << c << endl; 
    complex<double> d = polar(1.41421, -3.14152 / 4); 
    cout << d << endl;
```





##  STL Links.

https://stackoverflow.com/questions/5209602/pointer-arithmetic-ptr-or-ptr

