# Creational Patterns

## Abstract Factory
[[CppCon/Concurrency]]

https://www.youtube.com/watch?v=HcxhrAqEukc

https://refactoring.guru/design-patterns/abstract-factory

- Creates families of objects.

### Intent

- Provide **some interface** for *creating families* of *dependent objects* without specifying **concrete classes**
- The **concrete classes** of the **objects** should **not be specified**.

> A concrete class in Java is a type of subclass, which implements all the abstract method of its super abstract class which it extends to. It also has implementations of all methods of interfaces it implements.

### Problem

- **Instantiation** of *families* of *related objects*
  - Instatiation requires **hard coding** of *classes* $\to$ difficult to **modify** 
  - The **objects** are *related* to some *family* and **used together**, the *change* of family will **be cumbersome**.
  - Attempting to **add new family / new product**, may end up creating objects based on **multiple** scenarios using *if / switch* as the number of **families** and their **objects grow**.

### Solution

- Come up with some **method** to *create the family* of **objects**
- Seperate **the instantiation** of *objects* from **use**.

### Applications

1. Create a set of objects, which work together.
2. Supporting different families such as OS or product versions.
3. Creationg of **products** has to **be seperated** from *use*.
4. Interface to the **outside world**, there is **no requirement** to *hide implementation*
   -  Libraries
   -  Frameworks
   -  Plugins

### Structure

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220201150921603.png)

#### Classes involved

- ***AbstractFactory*** provides an **interface** for **creating product objects**.
- **ConcreteFactory** classes **derived** from the **AbstractFactory** implements the **methods** for **creating product objects**. 
  - Each **concrete class** represents **one family**, so we can say **ConcreteFactory1** has the **methods** to **create objects** for Series 1 product objects and ConcreteFactory2 has the methods to create objects for Series 2 product objects.
- ***AbstractProduct*** classes provides **interface** for **operations on Products.**
- ***Product*** classes are **concrete** **classes** derived from **AbstractProduct** classes to **implement the methods** for **Product**.
- ***Client*** uses the **Factory** and **Product**.

### How they work together

- `ConcreteFactory` $\to$ creates the **family** of products

  1. Creates **series 1 products** via *methods*
  2. Creates **series 2 products** via *methods*

- The *client* does **not have any information** of *concrete product classes* for **creating objects** as its in the method of `ConcreteFactory` class.

  - Each *concrete factory* is creating a set of **objects** for a **family** and *client* uses this
    - Thus **instantiation** becomes **seperate from use**.

  - Replacing family , easy , the **concrete factory** used at a **single place** and we obtain **another** *set of product* for a **family** by changing the **concrete factory**

- Adding *new product* 

  - Add methods for `AbstractFactory` / `ConcreteFactory` 
    - Thus think of **dynamically adding methods**
  - See here that each family requires **one** `ConcreteFactory` thus `ConreteFactory` implemented as **==singleton==**

#### Example

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220201154100641.png)

### Implementation

```c++
#include <iostream>

class ConfigurationManager {
public:
	virtual void UseConfigurationManager() = 0;
};

class UnisysConfigurationManager : public ConfigurationManager {
public:
	virtual void UseConfigurationManager() {
		std::cout << "Inside UnisysConfigurationManager::UseConfigurationManager() \n";
	}
};

class IBMConfigurationManager : public ConfigurationManager {
public:
	virtual void UseConfigurationManager() {
		std::cout << "Inside UnisysConfigurationManager::UseConfigurationManager() \n";
	}
};

class OperationManager {
public: 
	virtual void UseOperationManager() = 0;
};

class UnisysOperationManager : public OperationManager {

public:
	virtual void UseOperationManager() {
		std::cout << "Inside UnisysOperationManager::UseOperationManager() \n";
	}
};


class IBMOperationManager : public OperationManager {

public:
	virtual void UseOperationManager() {
		std::cout << "Inside IBMOperationManager::UseOperationManager() \n";
	}
};

class SystemManagementFactory {

public:
	virtual ConfigurationManager* CreateConfigurationManager() = 0;
	virtual OperationManager* CreateOperationManager() = 0;
};

class UnisysSMFactory : public SystemManagementFactory{
public:
	virtual ConfigurationManager* CreateConfigurationManager()  {
		return new UnisysConfigurationManager();
	}
	 
	virtual OperationManager* CreateOperationManager() {
		return new UnisysOperationManager();
	}
};
class IBMSMFactory : public SystemManagementFactory {
public:
	virtual ConfigurationManager* CreateConfigurationManager() {
		return new IBMConfigurationManager();
	}
	virtual OperationManager* CreateOperationManager() {
		return new IBMOperationManager();
	}
};

int main() {
    
    /* create abstract objects*/
	ConfigurationManager* cm;
	OperationManager* om;
	SystemManagementFactory* factory1 = new UnisysSMFactory;
	cm = factory1->CreateConfigurationManager();
	cm->UseConfigurationManager();
	om = factory1->CreateOperationManager();
	om->UseOperationManager();

	SystemManagementFactory* factory2 = new IBMSMFactory;
	cm = factory2->CreateConfigurationManager();
	cm->UseConfigurationManager();
	om = factory2->CreateOperationManager();
	om->UseOperationManager();
}
```

## Builder Design Pattern

https://refactoring.guru/design-patterns/builder

- Builder design pattern **used** to **create a complex object** in steps with **different representations**

### Intent

- Seperate the **construction** of a **complex object** from its **representation** so the **same construction process** can create **different representations**

### Problem

- Construction of a **complex object** which *requires different representations*, 
  - Sometimes complex **objects** are created **in steps** to make the **final object**
    - Create *components* then *assemble* into some **final product**
- Sometime it **is required** to have **different representations** 
  - Therefore **different components** are *required* for assembling a **particular product**
    - Thus having the **construction** and *representation together* will make it complex and **hard to modify** / **extend** for the *future*.

### Solution
```c++
#include iostream

int main(){
	std::cout << "hello world" <, s
}


```
- A **method** to create **complex objects** in **steps** 
- Construction process should be **seperate** from **representation of object**
- Provide the **way** for *different representations* of the **object**

### Applications

- A **complex** *object* has to **created** in *steps*.
- Same **construction** process of object required to **provide** *different representation* of **final object**.
- The **construction** *process* requires **separation** from *representation* 

### Structure

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220201231445580.png)

### Participant Classes

- `Builder` provides **interface** for *creating parts* of **object**
- `ConcreteBuilder` class is **derived** from **builder** and **implements** *methods* of **builder** for *creating parts of the object*
  - Assembles **the part** to *make a final product*
    - Has the **representation** of objects and **provides** the *interface* to get the **created object**.
- `Director` class has **builder** as *aggregation*, it uses **builder interface** for *constructing* the **object**
- `Product` represent **object** to be *created*.

### How they work together

- `Client` uses the **director** and **provides** the **information** for the *product* it **wants to create**.
  - Director has **construction logic** based on the **provided information**, and **uses builder interface** to *create parts of object* and *assembled* them for a **required product** 
    - The **builder provides** the `product` to `director`  through **its interface**.
- Can see the **construction process** is *separated* from **representation** of *object*.
  - The **internal structure** of *object* is *not known* to the *outside* world as its **with concrete builder only**.
- The **construction process** has a way to **create object** in parts and **well managed** by the **director**.
- The **new representation** of *object* will *require* adding **new concrete builder** which can **be easily added**
  - Also it **will be easy to modify** the **construction logic** and **representation of objects**
- Some *scenarios* $\to$ may **require access** of *intermediate object* and has to be **taken care through the builder**
  - See $\to$ there is **no interface** for *product* as **final objects** will be *quite different* and also well **known** to *client* as it **provides the input**
- **Builder** has *all the methods* for *creating parts* of *objects*, so the *required* ones have **to be implemented** in *concrete builder*.

## Singleton

- A **creational design pattern** that lets you **ensure** a class has a **single instance**
- Provides **==global access==** to this instance.

### Problem

- Ensure class has a **==single instance==**
  - A **shared resource** such as a **file** / **database**.
    1. Create object
    2. Decide to create new one
    3. **Obtain** the **one you create** instead of **making a new one**
  - This is **not possible** with a *regular constructor* as it **must** return **new objects**.
- Provide **==global access point==** to that **instance**
  - **Singleton** lets us **access** some **object** from **anywhere** in the **program**.
  - Protects the **instance** from being **overwritten** by **other code**.
- Dont want **problem one**
  - Have **within one class**
    - Especially if the **code depends on it**.

### Solution

1. Make **default constructor** *private* 
   - Prevents **other objects** from using `new` operator with the **singleton class**
2. Create **static creation** method to *act* as some **constructor**
   - Under hood $\to$ this method calls **private constructor** to *create an object* and **saves** it in a *static field*
     - All calls to this **method** returns the **cached object**.

- Can call the **static method** to access the class
  - The **same object** being *returned each time*

### Real world analogy

- Government 
  - Country has *one government*.

### Structure

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220202004912191.png)

### Pseudocode 

https://refactoring.guru/design-patterns/singleton#:~:text=the%20Singleton%20object.-,Pseudocode,-In%20this%20example

### Applications

- Single instance requirement
  - Single **database object** that is **shared by different parts of the program**.
- It *disables* any mean of **creating new objects** of a *class* except for the **special creation method**
  - This method creates either a **new object** or **return existing one** if *created.*
- Need **stricter** control over **global variables**.
- Can make it **a specific number of instances** by changing the `getInstance` logic.

### Implementation

1. Add Private **static field** to the **class** for storing the **singleton instance**
2. Declare a **public static creation** method for **getting** the **singleton instance**
3. Implement ***lazy initialisation*** within the **static method**
   - This returns the **instance** on *all subsequent calls*.
4. Make the **constructor** *of the class* **private** 
   - The **static method** of the *class* will *still be able to* **call the constructor** but the **not other objects**
5. Go over client code replace **direct calls** to the **singleton constructor** with **calls** to its **static creation method**

### Pros / Cons

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220202005556537.png)

### C++ Implementation

#### Thread Safe

```c++
/**
 * The Singleton class defines the `GetInstance` method that serves as an
 * alternative to constructor and lets clients access the same instance of this
 * class over and over.
 */
class Singleton
{

    /**
     * The Singleton's constructor/destructor should always be private to
     * prevent direct construction/desctruction calls with the `new`/`delete`
     * operator.
     */
private:
    static Singleton * pinstance_;
    static std::mutex mutex_;

protected:
    Singleton(const std::string value): value_(value)
    {
    }
    ~Singleton() {}
    std::string value_;

public:
    /**
     * Singletons should not be cloneable.
     */
    Singleton(Singleton &other) = delete;
    /**
     * Singletons should not be assignable.
     */
    void operator=(const Singleton &) = delete;
    /**
     * This is the static method that controls the access to the singleton
     * instance. On the first run, it creates a singleton object and places it
     * into the static field. On subsequent runs, it returns the client existing
     * object stored in the static field.
     */

    static Singleton *GetInstance(const std::string& value);
    /**
     * Finally, any singleton should define some business logic, which can be
     * executed on its instance.
     */
    void SomeBusinessLogic()
    {
        // ...
    }
    
    std::string value() const{
        return value_;
    } 
};

/**
 * Static methods should be defined outside the class.
 */

Singleton* Singleton::pinstance_{nullptr};
std::mutex Singleton::mutex_;

/**
 * The first time we call GetInstance we will lock the storage location
 *      and then we make sure again that the variable is null and then we
 *      set the value. RU:
 */
Singleton *Singleton::GetInstance(const std::string& value)
{
    std::lock_guard<std::mutex> lock(mutex_);
    if (pinstance_ == nullptr)
    {
        pinstance_ = new Singleton(value);
    }
    return pinstance_;
}

void ThreadFoo(){
    // Following code emulates slow initialization.
    std::this_thread::sleep_for(std::chrono::milliseconds(1000));
    Singleton* singleton = Singleton::GetInstance("FOO");
    std::cout << singleton->value() << "\n";
}

void ThreadBar(){
    // Following code emulates slow initialization.
    std::this_thread::sleep_for(std::chrono::milliseconds(1000));
    Singleton* singleton = Singleton::GetInstance("BAR");
    std::cout << singleton->value() << "\n";
}

int main()
{   
    std::cout <<"If you see the same value, then singleton was reused (yay!\n" <<
                "If you see different values, then 2 singletons were created (booo!!)\n\n" <<
                "RESULT:\n";   
    std::thread t1(ThreadFoo);
    std::thread t2(ThreadBar);
    t1.join();
    t2.join();
    
    return 0;
}
```

## Prototype

- Copy **existing objects** *without making* the **code dependent** on their **classes**.

### Problem

- Create an **exact copy** of *some object*
  - Create **new object** of the **same class**
    - Go over **each field** of the **original object** and *copy* the values to the **new object**.

-  Not all **objects** can be **copied** that way because some of the **object’s fields** may be **private** and **not visible** from **outside** of the **object itself**.

-  Since you have to **know** the **object’s class** to **create** a **duplicate**, your **code** becomes **dependent** on that class. 
- Sometimes you **only know the interface** that the **object** **follows**, but **not its concrete class**, when, for example, a **parameter** in a **method** **accepts** any **objects** that **follow some interface**.

### Solution

- The Prototype pattern **delegates** the **cloning** **process** to the **actual** **objects** that are **being cloned**. 
- The pattern **declares** a **common interface** for all **objects** that **support cloning**.
-  This **interface** **lets** you **clone an object** without **coupling** your **code** to the **class** of that **object**. Usually, **such an interface** **contains** just a **single** `clone` **method**.

---

- The **implementation** of the `clone` method is very **similar** in all **classes**. 
- The **method** creates **an object** of the **current** **class** and **carries** over **all of the field** values of the **old object** into the **new one**
- You can even **copy private fields** because **most programming languages** let objects **access private fields** of other **objects** that **belong** to the **same class**.

---

- An object that supports **cloning** is called a ***prototype***.
-  When your **objects** have **dozens of fields** and **hundreds of possible configurations**, **cloning** them might serve as an **alternative** to **subclassing**.

---

- Here’s how it works: you **create a set of objects**, **configured** in **various** ways.
- When you **need** an **object** like the one **you’ve configured**, you just **clone** a **prototype** instead of **constructing** a **new** **object** from **scratch**.

### Structure

#### Basic

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220202214558192.png)

#### Prototype registry implementation

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220202214656981.png)

### Pseudocode

https://refactoring.guru/design-patterns/prototype#:~:text=of%20the%20registry.-,Pseudocode,-In%20this%20example

### Applications

- Use when the **code** should **not depend** on the **concrete classes** of *objects* that you *need to copy*.
- This **happens** a *lot* when you **code works** with objects **passed** to you from **3rd party code** via *some interface*
  - The **concrete classes** of *these objects* are *unknown*, and you **couldn't** depend on them **even if you wanted to**.

- Use the **pattern** when you want to **reduce the number** of **subclasses** that only **differ** in the **way they initialize their respective** objects. Somebody could have **created** these **subclasses** to be able to **create** **objects** with a **specific** **configuration**.

- The **Prototype** **pattern** lets you use a **set** of **pre-built objects,** configured in **various ways**, as **prototypes**.
- Instead of **instantiating** a **subclass** that **matches** some **configuration**, the client can simply **look** for an **appropriate** **prototype** and clone it.

### How To implement

1. Create the **prototype** **interface** and declare the `clone` method in it. Or just **add** the **method** to **all classes** of an existing class **hierarchy**, if you **have one**.

2. A **prototype** class must **define** the **alternative constructor** that **accepts** an **object** of that **class** as an **argument**. 

   - The **constructor** must **copy** the **values** of all **fields** **defined** in the **class** from the **passed object** into the **newly** **created** **instance**.
   - If you’re **changing** a **subclass**, you must **call** the **parent constructor** to let the **superclass** **handle** the **cloning** of its **private** fields.

3. Cloning method **consists** of a **single line**

   - Running a `new` operator with the **prototypical** version of the **constructor**. 

     >  that every class must **explicitly** **override** the **cloning** **method** and use its **own class name** along with the `new` operator. **Otherwise**, the **cloning** method may **produce** an **object** of a parent class.

4. Optionally, **create** a **centralized** **prototype** **registry** to store a **catalog** of frequently used prototypes

### Pros and Cons

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203002631915.png)

### C++ Implementation

```c++
using std::string;

// Prototype Design Pattern
//
// Intent: Lets you copy existing objects without making your code dependent on
// their classes.

enum Type {
  PROTOTYPE_1 = 0,
  PROTOTYPE_2
};

/**
 * The example class that has cloning ability. We'll see how the values of field
 * with different types will be cloned.
 */

class Prototype {
 protected:
  string prototype_name_;
  float prototype_field_;

 public:
  Prototype() {}
  Prototype(string prototype_name)
      : prototype_name_(prototype_name) {
  }
  virtual ~Prototype() {}
  virtual Prototype *Clone() const = 0;
  virtual void Method(float prototype_field) {
    this->prototype_field_ = prototype_field;
    std::cout << "Call Method from " << prototype_name_ << " with field : " << prototype_field << std::endl;
  }
};

/**
 * ConcretePrototype1 is a Sub-Class of Prototype and implement the Clone Method
 * In this example all data members of Prototype Class are in the Stack. If you
 * have pointers in your properties for ex: String* name_ ,you will need to
 * implement the Copy-Constructor to make sure you have a deep copy from the
 * clone method
 */

class ConcretePrototype1 : public Prototype {
 private:
  float concrete_prototype_field1_;

 public:
  ConcretePrototype1(string prototype_name, float concrete_prototype_field)
      : Prototype(prototype_name), concrete_prototype_field1_(concrete_prototype_field) {
  }

  /**
   * Notice that Clone method return a Pointer to a new ConcretePrototype1
   * replica. so, the client (who call the clone method) has the responsability
   * to free that memory. I you have smart pointer knowledge you may prefer to
   * use unique_pointer here.
   */
  Prototype *Clone() const override {
    return new ConcretePrototype1(*this);
  }
};

class ConcretePrototype2 : public Prototype {
 private:
  float concrete_prototype_field2_;

 public:
  ConcretePrototype2(string prototype_name, float concrete_prototype_field)
      : Prototype(prototype_name), concrete_prototype_field2_(concrete_prototype_field) {
  }
  Prototype *Clone() const override {
    return new ConcretePrototype2(*this);
  }
};

/**
 * In PrototypeFactory you have two concrete prototypes, one for each concrete
 * prototype class, so each time you want to create a bullet , you can use the
 * existing ones and clone those.
 */

class PrototypeFactory {
 private:
  std::unordered_map<Type, Prototype *, std::hash<int>> prototypes_;

 public:
  PrototypeFactory() {
    prototypes_[Type::PROTOTYPE_1] = new ConcretePrototype1("PROTOTYPE_1 ", 50.f);
    prototypes_[Type::PROTOTYPE_2] = new ConcretePrototype2("PROTOTYPE_2 ", 60.f);
  }

  /**
   * Be carefull of free all memory allocated. Again, if you have smart pointers
   * knowelege will be better to use it here.
   */

  ~PrototypeFactory() {
    delete prototypes_[Type::PROTOTYPE_1];
    delete prototypes_[Type::PROTOTYPE_2];
  }

  /**
   * Notice here that you just need to specify the type of the prototype you
   * want and the method will create from the object with this type.
   */
  Prototype *CreatePrototype(Type type) {
    return prototypes_[type]->Clone();
  }
};

void Client(PrototypeFactory &prototype_factory) {
  std::cout << "Let's create a Prototype 1\n";

  Prototype *prototype = prototype_factory.CreatePrototype(Type::PROTOTYPE_1);
  prototype->Method(90);
  delete prototype;

  std::cout << "\n";

  std::cout << "Let's create a Prototype 2 \n";

  prototype = prototype_factory.CreatePrototype(Type::PROTOTYPE_2);
  prototype->Method(10);

  delete prototype;
}

int main() {
  PrototypeFactory *prototype_factory = new PrototypeFactory();
  Client(*prototype_factory);
  delete prototype_factory;

  return 0;
}
```

