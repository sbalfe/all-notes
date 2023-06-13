# Effective STL (Summary)

## Item 1 - Choose containers with care.

- **==sequence containers==** 
  1. Vector
  2. String
  3. deque
  4. list
  5. array
- **==associative containers==**
  - set
  - multiset
  - map
  - multimap
- **==non standard sequence==**
  - forward_list
  - rope
- **==non standard associative==**
  - hash_set
  - hash_multiset
  - hash_map
  - hash_multimap
- **==contiguous-memory==**
  - vector
  - string
  - deque rope
  - array
- **==node-based==**
  - lists
  - forward_list
  - Any standard associative container.

---

## Item 2 - Beware the illusion of the container (independent code)

- Only **sequence containers** support `push_back` / `push_front` 

- Only **associative containers** support `count` and `lower_bound` 

```c++
class Widget {
    
private:
	int _data;
public:
	Widget(int x) : _data(x) {}
    
    /* override equality operator take in the rhs compare to current widget.*/
	bool operator==(const Widget& rhs) { return this->data() == rhs.data(); }
	int data() const { return _data; }
};

typedef vector<Widget> WidgetContainer; 
typedef WidgetContainer::iterator WCIterator; /*WCIterator = vector<Widget>::iterator*/ 
	
/* universal initialisaiton of three widgets */
WidgetContainer vw{ Widget(1), Widget(2), Widget(3) };

/* create a new widget*/
Widget bestWidget(2);

/* vector<Widget>::iterator is i, we apply the std::find from the start to end using the new 
	widget as a locator
*/
WCIterator i = find(vw.begin(), vw.end(), bestWidget);

/* vector<widget>::difference_type > specific type to tell us the distance between iterators. */
WidgetContainer::difference_type distance = i - vw.cbegin(); // distance will be 1
```

## Item 3 - Make copying cheap and correct for objects in containers.

- Containers **store objects**
  - But the **stored** ones are **not exactly** what you give, they are a **copy**
- The **operations** later on the **objects** in **containeres** are often based on the **copy**
  - The **copy** is *implemented* by **objects** copy member *constructor* or *copy assignment operator*

```c++
class Widget {
private:
	int _data;
public:
	Widget(int x) : _data(x) {}
    
	/* take good care of cp ctor and cp assiganment definition, because they may get called frequently in STL     functions	Widget(const Widget& rhs) :_data(rhs.data()) {} */
    
    /* returns reference to widget taking in reference to a widget*/
	Widget& operator=(const Widget& rhs) {
		this->_data = rhs.data();
		return *this;
	}
	bool operator==(const Widget& rhs) { return this->data() == rhs.data(); }
	int data() const { return _data; }
};
```

- Better approach to **fix this** is **holding pointers** to *objects* instead of **objects themselves**

## Item 4 - Call empty instead of checking size() against zero

- Use `container.empty()` to **check** if **container** is **empty** instead of `container.size() == 0` 
  - Reason $\to$ empty is some **container time operation** for *all standard containers*
    - but for **some implementations** , size *takes linear time*.

## Item 5 - Prefer range member functions to their single element counterparts

- Say we **have three versions** of *range intersection*

- Common *initilisation*:

```c++
vector<int> v1 {99,99};
const int numValues = 10;
int data[numValues] = {0,1,2,3,4,5,6,7,8,9};
```

1. Version 1

   ```c++
   v1.insert(v1.begin, data, data + numValues);
   ```

2.  Version 2

   ```c++
   vector<int>::iterator insertLoc(v1.begin());
   
   for (int i = 0; i < numValues; ++i){
       insertLoc = v1.insert(insertLoc, data[i]);
   }
   ```

3. Version 3

   ```c++
   v1.resize(20); /* copy cannot assign values to the place where there is nothing before*/
   std::copy(data, data + numValues, v1.begin());
   ```



- Regarding **efficiency**

  - version 2 / 3 are **almost the same** when version 3 does **not need memory reallocation** for `resize()` being called.
    - Compared to version 1 $\to$ They have these **disadvantages**:
      1. More times of **uneccessary function calls**
      2. More times of **elements shift**
      3. More **frequently** *memory allocations*

  

- Container **construction** $\to$ *all standard containers* provide the method below:

  ```c++
  container::container(InputIterator begin, InputIterator end);
  ```
  
- **Range insertion** $\to$ *All standard containers provide method below* 

  ```c++
  container::insert(iterator position, InputIterator begin, InputIterator end);
  ```
  
- Since **Associative** containers *have their own* method **to calculate** *insertion* position, usually the **first argument** can be **omitted**.

  ```c++
  void container::container(InputIterator begin, InputIterator end);
  ```
  
- **Range erasure** $\to$ *all standard containers* provide *range erasure functions*, but **return time** are *different*.

  - All standard containers provide **range erasure functions**
    - But return **time are different**.

```c++
/* Sequence Container*/

iterator container::erase(iterator begin , iterator end); /* returns iterator following last removed element*/

/* associative container */

void container::erase(iterator begin , iterator end)
```

- **Range assignment**
  - All *containers provide* method *below*

```c++
void container::assign(InputIterator begin, InputIterator end);
```

---

- `resize()` $\to$ only affects `size()` as it adds or removes **the number of elements** to the **vector** to **make it given size**.
- `reserve()` only **allocates memory** but leaves it **uninitiliazed**, only affects `capacity()`, there is **no value** for the **objects** as nothing is added an **no reallocation** as it **was done *in advance***.
- *Use case dependent*.

## Item 6 - Be alert for C++ most vexing parse

- Compiler tends to **recognize** everything as a **function declaration**

```c++
class Widget {
private:
	int _data;
public:
	Widget() { std::cout << "default ctor\n"; }
	Widget(int x) : _data(x) {}
	Widget(const Widget& rhs) :_data(rhs.data()) {}
	Widget& operator=(const Widget& rhs) {
		this->_data = rhs.data();
		return *this;
	}
	bool operator==(const Widget& rhs) { return this->data() == rhs.data(); }
	int data() const { return _data; }
};
		
	Widget w(); // this will be regarded as a function declaration , which returns Widget and takes no arguments instead of default ctor.
```

- Another example

```c++
ifstream dataFile("ints.dat");
list<int> data(istream_iterator<int>(dataFile), istream_iterator<int>()); // warning! this doesn't do what you think it does
```

- The brackets are ignored in the above one and thus it does not call any constructor but rather just is a function declaration.

## Item 7 - When using containers of newed pointers, remember to delete the pointers before the container is destroyed.

- For most **standard containers**
  - When its **out of scope** $\to$ elements may **be destroyed** by **its destructor** so *we only focus* on **containers** of *pointers* here

```c++
class Widget {
private:
	int _data;
public:
	Widget(int x) : _data(x) {}
	Widget(const Widget& rhs) :_data(rhs.data()) {}
	Widget& operator=(const Widget& rhs) {
		this->_data = rhs.data();
		return *this;
	}
	bool operator==(const Widget& rhs) { return this->data() == rhs.data(); }
	int data() const { return _data; }
};
		
vector<Widget*> vwp;
```

- **Worst method**, may work but **not type safe** and **not exception safe**

```c++
for (int i = 0; i < 10; ++i) {
  	vwp.push_back(new Widget(i));
}

for (vector<Widget*>::iterator it = vwp.begin(); it != vwp.end(); ++it) {
  	delete *it;
}
```

- This works **sometimes** but its **neither exception safe** nor *clear*
- **Better method**, this is **clear** and **type safe** but **not exception safe**.

```c++
struct DeleteObject{
	  void operator()(const Widget* ptr) const {
        delete ptr;
    }
};

for (int i = 0; i < 10; ++i) {
  	vwp.push_back(new Widget(i));
}
for_each(vwp.begin(), vwp.end(), DeleteObject());
vwp.clear();
```

- **Best method** $\to$ use `shared_ptr` for **everything**
  - No **need** to *delete them explicitly* as **shared pointer** will *deal* with it *cleverly*

```c++
typedef std::shared_ptr<Widget> SPW;
vector<SPW> svwp;

for (int i = 0; i < 10; ++i) {
    svwp.push_back(make_shared<Widget>(i));
}
```

## Item 8 - Never create containers of auto_ptrs - COAPs

- When `auto_ptr` is **copied** its , **it actually** *got* *transferred*
  - Means the *original* `auto_ptr` will become `null`
    - This **attribute** makes it hard to use when **certain algorithms** are called with lots of **copy actions** occuring.
- Mentioned in item 7 $\to$ `shared_ptr` always **must be preferred**.

> Copying an `auto_ptr` copies the pointer and transfers ownership to the destination: both copy construction and copy assignment of `auto_ptr` modify their right-hand arguments, and the "copy" is not equal to the original. Because of these unusual copy semantics, `auto_ptr` may not be placed in standard containers. [std::unique_ptr](https://en.cppreference.com/w/cpp/memory/unique_ptr) is preferred for this and other uses. (since C++11)
>
> cpp reference- https://en.cppreference.com/w/cpp/memory/auto_ptr#:~:text=Copying%20an%20auto_ptr,since%20C%2B%2B11)

## Item 9 - Choose carefully among the erasing options

- The **reason** why *erasure* is so **subtle** is that it may **invalidate** the *iterator*
  - Therefore we can **never erase elements** *without consideration*.

- To **eliminate** *all objects* in some *container* $\to$ that have some **value** 

  - For **contiguous memory container** $\to$ use *erase / remove idiom*

    - The `std::remove` Transforms the range `[first,last)` into a range with all the elements that compare equal to val removed, and returns an iterator to the new end of that range.
    - `erase` destroys the **locations** that are **saved** by **remove**. 

  ```c++
  vector<int> vec1{ 0,1,2,3,4,5,6,7,8,9 };
  vec1.erase(std::remove(vec1.begin(), vec1.end(), 3), vec1.end());   // work for lists too, but not as efficient as below
  
  /*
  The function cannot alter the properties of the object containing the range of elements (i.e., it cannot alter the size of an array or a container): The removal is done by replacing the elements that compare equal to val by the next element that does not, and signaling the new size of the shortened range by returning an iterator to the element that should be considered its new past-the-end element.
  */
  ```
  
  - For *list*:
  
  ```c++
  list<int> list1 {0,1,2,3,4,5,6,7,8,9};
  list1.remove(3);
  ```
  
  - For *associative container* , it does **not make any sense** to *remove*, rather we **use erase**
  
  ```c++
  set<int> set1 {1,2,3,4,5};
  set1.erase(3); // takes log time
  ```
  

---

- To **eliminate all objects** in *container* that *satisfy* a **particular** *predicate*

  - For *contiguous-memory container*

  ```c++
  vector<int> vec2{ 0,1,2,3,4,5,6,7,8,9 };
  
  /* work for lists too, but not as efficient as below */
  vec2.erase(std::remove_if(vec2.begin(), vec2.end(), badValue), vec2.end()); 
  ```

  - For *list*

  ```c++
  list<int> list2{ 0,1,2,3,4,5,6,7,8,9 };
  list2.remove_if(badValue);
  ```

  - For *associative containers* 

    - Easy, copying all the elements that are **not being removed**.

      ```c++
      set<int> set2{ 1,2,3,4,5 }, set2_goodvalues;
      remove_copy_if(set2.begin(), set2.end(), inserter(set2_goodvalues, set2_goodvalues.end()), badValue);
      set2.swap(set2_goodvalues);
      ```

    - Efficient, being sure to **postincrement** your **iterator** when you pass it to *erase*

      ```c++
      set<int> set3{ 1,2,3,4,5 };
      for (set<int>::iterator it = set3.begin(); it != set3.end();) {
        if (badValue(*it))
            set3.erase(it++); // to avoid invalidation of it once erased   else
            ++it;
      }
      ```

- To loop in **containers**

  - To loop in *contiguous memory container*

    - Loop to erase with log, ensure to update the iterator with erases return value each time we call it

    ```c+
    vector<int> vec3{ 0,1,2,3,4,5,6,7,8,9 };
    for (vector<int>::iterator it = vec3.begin(); it != vec3.end();)
        if (badValue(*it)) {
            std::cout << "Erasing:" << *it << std::endl;
            it = vec3.erase(it);
        }
        else
            ++it;
            
    ```

    - No worries to **loop** in *list* as it **never invalidates** *iterators*
    - To *loop* in associative containers, just use the same method above as **efficient erasure**
