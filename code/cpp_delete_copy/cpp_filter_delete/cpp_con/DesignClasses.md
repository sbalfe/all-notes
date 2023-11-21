
# Designing Guidelines for Classes

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125034546161.png)

- Change fucks code bruh.
  - Must design software to be dynamic and able to frequently change.

---

- **Dependencies** are the core reason for this dynamic lock
  - This makes it harder to **change** shit.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125034723246.png)

---

> Design classes for **==easy change==**
>
> Design classes for **==easy extensions==**

---

### Design For Readability

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125034841655.png)

```C++
/* template for STL array */

template<
	class T,
	std::size_t N // wtf is this cunting name, name Size u cunt.
> struct array;
```

```c++
/* template for std::vector<T>*/

template<
	class T,
	class Allocator = std::allocator<T>
> class vector; /* container or numerical vector ??? */
{
    public:
    	// ...
    	[[nodiscard]] constexpr bool empty() const noexcept; /* is empty an action / query > query as bool is 																true or false, say isempty either*/
    	// 
}
```

- Naming is a key concept for class design.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125035348788.png)

### Shapes *toy problem*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125035411077.png)

#### Class hierarchy

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125035438221.png)

- Contains a **pure virtual function** called `draw()`. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125035641192.png)

- Adding all the **draw implementation** within a **specific** class we create some *dependency* 
  - Be this some *library* such as OpenGL.
- Add **more derivations** in the classes with more **specific implementations** and ensure **generic** shapes are not implementing it.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125035843792.png)

- Say things change, we may add **serialize** virtual function to our class.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125035950395.png)

- Add another **layer** to **implement** this method in its own way

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125040016001.png)

- We **duplicate** this and it becomes **very repeated** in parts and **hard to change**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125040051942.png)

- This complete fucker is pretty stupid.

[[effective-c-55-specific-ways-to-improve-your-programs-and-designs-third-edition-3rd-edition-0321334876-9780321515827-032151582x-9780321334879_compress.pdf]]

---

- Using **inheritance** in some *naive nature* would lead to 
  1. **many derived classes**.
  2. **retarded names**
  3. *deep* inheritance **hierarchies**
  4. **duplication**
  5. *Impossible extensions*
  6. **Impede maintenance** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125040135459.png)

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125040253360.png)

- Use OO correctly if you are using it you cunt.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125040325859.png)

#### Solution - Design principles / Patterns.

##### Single responsibility principle (SRP)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125040515268.png)

- Alternatively speaking:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125040545459.png)

- Think of it as **==separation of concerns==** 

  1. **High cohesion** / **Low coupling**

     - Cohesion is the content and purpose of a class a whole
     - coupling refers to the shared links between the contents of the class.

  2. **Orthogonality**

     > In [computer programming](https://en.wikipedia.org/wiki/Computer_programming), **orthogonality** means that operations change just one thing without affecting others.[[1\]](https://en.wikipedia.org/wiki/Orthogonality_(programming)#cite_note-1) The term is most-frequently used regarding assembly instruction sets, as [orthogonal instruction set](https://en.wikipedia.org/wiki/Orthogonal_instruction_set).

##### Open-Closed Principle (OCP)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125040806267.png)

- Easy to add to the software we create using oop.
  - Possibly simply by **just adding more code** and *not changing existing code* 

##### Do not repeat yourself (DRY)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125041036492.png)

- Repetition can lead to **bugs / inconsistency** in style of code.
