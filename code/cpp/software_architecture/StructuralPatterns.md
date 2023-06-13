# Structural patterns

## Adapter

- **Adapter** is a structural design pattern that **allows** objects with **incompatible** **interfaces** to **collaborate**.

### Problem

- Creating a **stock market monitor**
  - Downloads **stock data** from sources in **XML** format and displays **charts** and **diagrams** for the **user**.
- Decide to **improve** by **integrating** a *smart* *3rd party analytics library*
  - The **analytics** only **works** with *data* in *JSON* format.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203003535190.png)

- Change the **library** to work with **XML** 
  - This may **break existing code** and you may *not have source code access* in the first place thus making the **approach impossible**.

### Solution

- You can create an *adapter*. This is a **special** **object** that **converts** the **interface** of one **object** so that another object can understand it.

- An **adapter** **wraps** one of the **objects** to **hide** the **complexity** of **conversion** happening **behind** the scenes. The **wrapped** **object** isn’t even **aware** of the **adapter**. For **example**, you can **wrap** an **object** that **operates** in **meters** and **kilometres** with an **adapter** that **converts** all of the **data** to **imperial** units such as **feet and miles**.

- Adapters can **not only convert data** into **various** **formats** but can also **help** **objects** with **different** **interfaces** collaborate. Here’s how it works:

1. The **adapter** gets an **interface**, **compatible** with one of the **existing** objects.
2. Using this **interface**, the **existing** **object** can safely call the **adapter’s** methods.
3. Upon **receiving** a **call**, the **adapter** **passes** the **request** to the **second** object, but in a **format** and **order** that the **second** object expects.

- **Sometimes** it’s even **possible** to create a **two-way adapter** that can **convert** the **calls** in **both** directions.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203004112134.png)

- To solve the **dilemma** of incompatible formats, you can create **XML-to-JSON** **adapters** for every **class** of the **analytics** library that your code works with directly.
-  Then you **adjust your code to communicate** with the **library** only **via these adapters**.
-  When an **adapter** receives a **call**, it **translates** the incoming **XML** data into a **JSON** **structure** and **passes** the call to the **appropriate methods** of a **wrapped** **analytics** object.

### Structure

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203004656517.png)

1. Client is a class containing logic of the program
2. client describes **protocol** that other classes follow to be **able to collaborate**  with the **client code**
3. **Service** is some *useful class*
   - Client can **not use this class directly** as its an **incompatible interface**
4. **Adapter** is *a class* that can work with **both the client** and **the service**
   - Implements the **client interface**
     - While **wrapping the service object** 
   - Adapter receives **calls** from the **client** via the **adapter interface** and *translates* to **calls** to the **wrapped service object** in a *format* it **can understand**.
5. Client code does **not get coupled** to the **concrete adapter**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203005118349.png)

### Pseudocode

https://refactoring.guru/design-patterns/adapter#:~:text=existing%20client%20class.-,Pseudocode,-This%20example%20of

### Applications

- Use the **adapter class** when you **want** to use some **existing class**
  - But its **interface** is *not compatible* with the **rest of the code**.
- `test` 


-  The **Adapter** pattern lets you create a **middle-layer** class that **serves** as a **translator** between your **code** and a **legacy** class, a **3rd-party class** or any **other class with a weird interface**.

- Use the pattern when you want to **reuse several existing subclasses** that **lack** some **common** **functionality** that **can’t** be added to the **superclass**.

- You could **extend** each **subclass** and put the **missing functionality** into new **child** **classes**. However, you’ll need to **duplicate** the **code** **across** all of **these new classes,** which is a **code smell**

### Implementation Explanation

1. Make sure that you have at **least two classes** with **incompatible** interfaces:
   - A useful ***service*** class, which you **can’t change** (often **3rd-party**, **legacy** or with **lots of existing dependencies**).
   - One or **several *client* classes** that would **benefit from using the service class**.
2. Declare the **client** **interface** and **describe how clients** **communicate** with the **service**.
3. Create the **adapter** **class** and **make** it **follow** the **client** interface. **Leave** all the **methods** **empty** for now.
4. Add a **field** to the **adapter** **class** to store a **reference** to the **service** **object**. The common **practice** is to **initialize** this **field** via the **constructor**, but **sometimes** it’s more **convenient** to **pass** it to the **adapter** when **calling** its **methods**.
5. One by one, **implement** all **methods** of the **client** **interface** in the **adapter** class. The adapter should **delegate** most of the **real work** to the **service** **object**, **handling** only the **interface** or **data** format conversion.
6. **Clients** should use the **adapter** via the **client** interface. This will let you **change** or **extend** the **adapters** without **affecting** the **client code**.

### Pros and Con

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203054124204.png)

### C++ Implementation

```c++
/**
 * The Target defines the domain-specific interface used by the client code.
 */
class Target {
 public:
  virtual ~Target() = default;

  virtual std::string Request() const {
    return "Target: The default target's behavior.";
  }
};

/**
 * The Adaptee contains some useful behavior, but its interface is incompatible
 * with the existing client code. The Adaptee needs some adaptation before the
 * client code can use it.
 */
class Adaptee {
 public:
  std::string SpecificRequest() const {
    return ".eetpadA eht fo roivaheb laicepS";
  }
};

/**
 * The Adapter makes the Adaptee's interface compatible with the Target's
 * interface.
 */
class Adapter : public Target {
 private:
  Adaptee *adaptee_;

 public:
  Adapter(Adaptee *adaptee) : adaptee_(adaptee) {}
  std::string Request() const override {
    std::string to_reverse = this->adaptee_->SpecificRequest();
    std::reverse(to_reverse.begin(), to_reverse.end());
    return "Adapter: (TRANSLATED) " + to_reverse;
  }
};

/**
 * The client code supports all classes that follow the Target interface.
 */
void ClientCode(const Target *target) {
  std::cout << target->Request();
}

int main() {
  std::cout << "Client: I can work just fine with the Target objects:\n";
  Target *target = new Target;
  ClientCode(target);
  std::cout << "\n\n";
  Adaptee *adaptee = new Adaptee;
  std::cout << "Client: The Adaptee class has a weird interface. See, I don't understand it:\n";
  std::cout << "Adaptee: " << adaptee->SpecificRequest();
  std::cout << "\n\n";
  std::cout << "Client: But I can work with it via the Adapter:\n";
  Adapter *adapter = new Adapter(adaptee);
  ClientCode(adapter);
  std::cout << "\n";

  delete target;
  delete adaptee;
  delete adapter;

  return 0;
}
```

## Bridge

- **Bridge** is a structural design pattern that lets you **split** a **large** **class** or a set of **closely related classes** into two **separate** **hierarchies**—**abstraction** and **implementation**—which can be **developed** **independently** of **each** other.

### Problem

- Say you have a geometric `Shape` class with a pair of subclasses: `Circle` and `Square`. 
- You want to **extend** this **class** **hierarchy** to **incorporate** **colors**, so you **plan** to **create** `Red` and `Blue` shape **subclasses**. However, since you already **have two subclasses**, you’ll need to create f**our class combinations** such as `BlueCircle` and `RedSquare`.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203054723209.png)

- Adding **new shape types** and **colors** to the **hierarchy** will grow it exponentially.
-  For example, to **add a triangle** shape you’d **need to introduce two subclasses**, one for **each** **color**. 
- Adding a **new color** would **require** creating **three subclasses**, one for **each shape type**. The **further** we go, the **worse it becomes**.

### Solution

- This problem occurs **because we’re trying** to **extend** the **shape** **classes** in two **independent dimensions**: by **form** and by **color**. That’s a very common **issue** with **class inheritance**.

- The Bridge **pattern** attempts to **solve this problem** by **switching** from **inheritance** to the **object composition**. 
- What this means is that **you extract one of the dimensions** into a **separate** **class** **hierarchy**, so that the **original classes** will **reference** an **object** of the **new hierarchy**, instead of **having** all of its **state** and **behaviours** within **one class**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203055105227.png)

- Following this approach, we can e**xtract the color-related** code into its **own class** with **two subclasses**: `Red` and `Blue`. 
- The `Shape` class then **gets a reference field** pointing to **one of the color objects**. 
- Now the **shape can delegate** any **color-related** work to the **linked color object**. 
- That **reference** will act as a **bridge** between the `Shape` and `Color` classes. From now on, **adding new colors** won’t require changing the **shape hierarchy**, and vice versa.

### Abstraction / Implementation

- *Abstraction* (also called *interface*) is a **high-level control layer** for some **entity**. This layer isn’t **supposed** to **do any real work** on its own. It should **delegate** the **work** to the ***implementation*** layer (also called *platform*).
- When talking about real applications, the **abstraction** can be **represented by a graphical user interface** (GUI), and the **implementation** could be the **underlying operating system** code (API) which the **GUI layer calls** in **response** to user **interactions**.

### Structure

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203055750135.png)

### Pseudocode

https://refactoring.guru/design-patterns/bridge#:~:text=the%20implementation%20objects.-,Pseudocode,-This%20example%20illustrates

### Applications

- Use the Bridge pattern when you want to **divide** and **organize** a **monolithic** **class** that has **several** **variants** of some functionality (for **example**, if the class can work **with various database servers)**.
- The **bigger** a **class** **becomes**, the **harder** it is to **figure** out **how it works**, and the **longer** it takes to **make a change**. The changes made to **one of the variations** of **functionality** may **require making changes** across the **whole class**, which often **results** in **making errors** or not **addressing** some **critical side effects**.

> The Bridge pattern lets you **split the monolithic class** into **several class hierarchies**. After this, you can **change the classes** in **each hierarchy** **independently** of the **classes in the others**. This approach **simplifies code** maintenance and **minimizes the risk of breaking existing code**.

- Use the pattern when you **need to extend a class** in several **orthogonal** (**independent**) **dimensions**.

  > The Bridge suggests that you **extract a separate class hierarchy** for **each** of the **dimensions**. The original **class delegates** the **related work** to the **objects belonging** to those **hierarchies** instead of **doing everything on its own**.

-  Although it’s optional, the **Bridge** **pattern** lets you **replace** the **implementation** object inside the **abstraction**. It’s as **easy** as **assigning** a new **value** to a **field**.

> *By the way, this **last item** is the **main reason** why so many people **confuse** the **Bridge** with the strategy pattern. Remember that a **pattern** is more than just a **certain** way to **structure** your classes. It may also **communicate** intent and a **problem being addressed***

### Implementation Explanation

1. Identify the **orthogonal dimensions** in your classes. These **independent** **concepts** could be: **abstraction**/**platform**, **domain**/**infrastructure**, **front**-**end**/**back**-**end**, or **interface**/**implementation**.
2. See what **operations** the **client** **needs** and **define** them in the **base abstraction class**.
3. Determine the **operations** available on all **platforms**. Declare the ones that the **abstraction needs** in the **general** **implementation** interface.
4. For all **platforms** in your **domain** create **concrete implementation classes**, but make sure they all **follow the implementation** interface.
5. Inside the **abstraction** class, add a **reference field** for the **implementation** type. The **abstraction** **delegates** most of the work to the **implementation** object that’s **referenced** in that **field**.
6. If you have **several** **variants** of **high**-**level** **logic**, create **refined abstractions** for **each** **variant** by **extending** the **base** **abstraction** class.
7. The **client code** should **pass** an **implementation object** to the **abstraction’s** **constructor** to **associate** **one** with the **other**. **After** that, the **client** can **forget** about the **implementation** and **work** only with the abstraction object.

### Pros and Cons

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203061909883.png)

### C++ Implementation

```c++
/**
 * The Implementation defines the interface for all implementation classes. It
 * doesn't have to match the Abstraction's interface. In fact, the two
 * interfaces can be entirely different. Typically the Implementation interface
 * provides only primitive operations, while the Abstraction defines higher-
 * level operations based on those primitives.
 */

class Implementation {
 public:
  virtual ~Implementation() {}
  virtual std::string OperationImplementation() const = 0;
};

/**
 * Each Concrete Implementation corresponds to a specific platform and
 * implements the Implementation interface using that platform's API.
 */
class ConcreteImplementationA : public Implementation {
 public:
  std::string OperationImplementation() const override {
    return "ConcreteImplementationA: Here's the result on the platform A.\n";
  }
};
class ConcreteImplementationB : public Implementation {
 public:
  std::string OperationImplementation() const override {
    return "ConcreteImplementationB: Here's the result on the platform B.\n";
  }
};

/**
 * The Abstraction defines the interface for the "control" part of the two class
 * hierarchies. It maintains a reference to an object of the Implementation
 * hierarchy and delegates all of the real work to this object.
 */

class Abstraction {
  /**
   * @var Implementation
   */
 protected:
  Implementation* implementation_;

 public:
  Abstraction(Implementation* implementation) : implementation_(implementation) {
  }

  virtual ~Abstraction() {
  }

  virtual std::string Operation() const {
    return "Abstraction: Base operation with:\n" +
           this->implementation_->OperationImplementation();
  }
};
/**
 * You can extend the Abstraction without changing the Implementation classes.
 */
class ExtendedAbstraction : public Abstraction {
 public:
  ExtendedAbstraction(Implementation* implementation) : Abstraction(implementation) {
  }
  std::string Operation() const override {
    return "ExtendedAbstraction: Extended operation with:\n" +
           this->implementation_->OperationImplementation();
  }
};

/**
 * Except for the initialization phase, where an Abstraction object gets linked
 * with a specific Implementation object, the client code should only depend on
 * the Abstraction class. This way the client code can support any abstraction-
 * implementation combination.
 */
void ClientCode(const Abstraction& abstraction) {
  // ...
  std::cout << abstraction.Operation();
  // ...
}
/**
 * The client code should be able to work with any pre-configured abstraction-
 * implementation combination.
 */

int main() {
  Implementation* implementation = new ConcreteImplementationA;
  Abstraction* abstraction = new Abstraction(implementation);
  ClientCode(*abstraction);
  std::cout << std::endl;
  delete implementation;
  delete abstraction;

  implementation = new ConcreteImplementationB;
  abstraction = new ExtendedAbstraction(implementation);
  ClientCode(*abstraction);

  delete implementation;
  delete abstraction;

  return 0;
}
```

## Composite

- **Composite** is a structural **design** **pattern** that lets you **compose objects** into **tree structures** and then **work** with these **structures** as if **they were individual objects**.

### Problem

- Using the Composite pattern makes sense only when the **core model** of your **app** can be **represented as a tree**.

- For example, **imagine** that you have **two types of objects**: `Products` and `Boxes`.

  -  A `Box` can contain several `Products` as well as a number of smaller `Boxes`. These little `Boxes` can also hold some `Products` or even smaller `Boxes`, and so on.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203062618225.png)

- You could try the direct approach: **unwrap** all the boxes, go over all the **products** and then **calculate** the total.
-  That would be **doable** in the real world; but in a **program**, it’s not as **simple as running a loop**. You have to **know** the **classes** of `Products` and `Boxes` you’re **going** through, the **nesting** **level** of the **boxes** and other **nasty** **details** beforehand.
-  All of this makes the **direct approach** either too **awkward** or even impossible.

### Solution

- The Composite pattern **suggests** that you work with `Products` and `Boxes` through a **common interface** which **declares** a **method** for **calculating the total price**.

- For a product, it’d simply **return** the product’s price.

-  For a **box**, it’d **go over each item** the box **contains**, ask its **price** and then **return** a **total** for this box. 

  - If one of these **items** were a **smaller** box, that **box** would also **start** **going** over its **contents** and **so** on, until the **prices** of all **inner components** were calculated. A box could even **add some extra cost** to the **final** price, such as **packaging** cost.

    > Run behaviour recursively over all components of an object tree

- The greatest **benefit of this approach** is that you don’t **need** to **care** about the **concrete classes** of objects that **compose** the **tree**.
-  You don’t need to **know whether an object** is a **simple** **product** or a **sophisticated** box. You can **treat** them all the **same** via the **common** **interface**. When you **call a method**, the **objects** **themselves** pass the **request down the tree**.

### Structure

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203063016992.png)

### Pseudocode

https://refactoring.guru/design-patterns/composite#:~:text=of%20the%20tree.-,Pseudocode,-In%20this%20example

### Applications

1. Use the **Composite pattern** when you have to **implement a tree-like object structure**.
   - The **Composite** pattern **provides** you with **two basic element types** that share a **common** **interface**:
     -  **Simple** leaves and **complex** containers. A **container** can be **composed** of both **leaves** and other **containers**. This lets you **construct** a nested **recursive** **object** **structure** that **resembles** a tree.

2.  Use the **pattern** when you want the **client** **code** to treat both **simple** and **complex** **elements** uniformly.
   - All **elements** defined by the **Composite** **pattern** share a **common** **interface**. Using this **interface**, the **client** doesn’t have to **worry** about the **concrete** **class** of the **objects** it works with.

### Implementation

1. Make sure that the **core model of your app** can be **represented** as a **tree structure**. Try to **break** it **down** into simple **elements** and **containers**. **Remember** that **containers** must be able to **contain both simple elements** and other **containers**.

2. **Declare** the **component** **interface** with a **list of methods** that make **sense** for both **simple** and **complex** components.

3. Create a **leaf** **class** to **represent** **simple** elements. A program may have **multiple** **different** **leaf** classes.

4. Create a **container class** to represent **complex elements**. 

   - In this class, provide an **array field** for **storing** **references** to **sub**-**elements**.
     - The **array** must be able to **store both leaves** and **containers**, so make **sure it’s declared** with the **component** **interface** type.

   > While **implementing** the **methods** of the **component** interface, **remember** that a **container** is supposed to be **delegating** **most** of the **work** to **sub**-**elements**.

5. Finally, **define** the **methods** for **adding** and **removal** of **child** **elements** in the **container**.

   > Keep in mind that these **operations** can be **declared** in the **component** interface. 
   >
   > - This would **violate** the *==Interface Segregation Principle==* because the **methods** will be **empty** in the **leaf** class.
   >   - However, the client will be able to **treat** all the **elements** **equally**, even when **composing** the **tree**.

### Pros and Cons

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220203064036814.png)

```c++
#include <algorithm>
#include <iostream>
#include <list>
#include <string>
/**
 * The base Component class declares common operations for both simple and
 * complex objects of a composition.
 */
class Component {
  /**
   * @var Component
   */
 protected:
  Component *parent_;
  /**
   * Optionally, the base Component can declare an interface for setting and
   * accessing a parent of the component in a tree structure. It can also
   * provide some default implementation for these methods.
   */
 public:
  virtual ~Component() {}
  void SetParent(Component *parent) {
    this->parent_ = parent;
  }
  Component *GetParent() const {
    return this->parent_;
  }
  /**
   * In some cases, it would be beneficial to define the child-management
   * operations right in the base Component class. This way, you won't need to
   * expose any concrete component classes to the client code, even during the
   * object tree assembly. The downside is that these methods will be empty for
   * the leaf-level components.
   */
  virtual void Add(Component *component) {}
  virtual void Remove(Component *component) {}
  /**
   * You can provide a method that lets the client code figure out whether a
   * component can bear children.
   */
  virtual bool IsComposite() const {
    return false;
  }
  /**
   * The base Component may implement some default behavior or leave it to
   * concrete classes (by declaring the method containing the behavior as
   * "abstract").
   */
  virtual std::string Operation() const = 0;
};
/**
 * The Leaf class represents the end objects of a composition. A leaf can't have
 * any children.
 *
 * Usually, it's the Leaf objects that do the actual work, whereas Composite
 * objects only delegate to their sub-components.
 */
class Leaf : public Component {
 public:
  std::string Operation() const override {
    return "Leaf";
  }
};
/**
 * The Composite class represents the complex components that may have children.
 * Usually, the Composite objects delegate the actual work to their children and
 * then "sum-up" the result.
 */
class Composite : public Component {
  /**
   * @var \SplObjectStorage
   */
 protected:
  std::list<Component *> children_;

 public:
  /**
   * A composite object can add or remove other components (both simple or
   * complex) to or from its child list.
   */
  void Add(Component *component) override {
    this->children_.push_back(component);
    component->SetParent(this);
  }
  /**
   * Have in mind that this method removes the pointer to the list but doesn't
   * frees the
   *     memory, you should do it manually or better use smart pointers.
   */
  void Remove(Component *component) override {
    children_.remove(component);
    component->SetParent(nullptr);
  }
  bool IsComposite() const override {
    return true;
  }
    
  /**
   * The Composite executes its primary logic in a particular way. It traverses
   * recursively through all its children, collecting and summing their results.
   * Since the composite's children pass these calls to their children and so
   * forth, the whole object tree is traversed as a result.
   */
    
  std::string Operation() const override {
    std::string result;
    for (const Component *c : children_) {
      if (c == children_.back()) {
        result += c->Operation();
      } else {
        result += c->Operation() + "+";
      }
    }
    return "Branch(" + result + ")";
  }
};
/**
 * The client code works with all of the components via the base interface.
 */
void ClientCode(Component *component) {
  // ...
  std::cout << "RESULT: " << component->Operation();
  // ...
}

/**
 * Thanks to the fact that the child-management operations are declared in the
 * base Component class, the client code can work with any component, simple or
 * complex, without depending on their concrete classes.
 */
void ClientCode2(Component *component1, Component *component2) {
  // ...
  if (component1->IsComposite()) {
    component1->Add(component2);
  }
  std::cout << "RESULT: " << component1->Operation();
  // ...
}

/**
 * This way the client code can support the simple leaf components...
 */

int main() {
  Component *simple = new Leaf;
  std::cout << "Client: I've got a simple component:\n";
  ClientCode(simple);
  std::cout << "\n\n";
  /**
   * ...as well as the complex composites.
   */

  Component *tree = new Composite;
  Component *branch1 = new Composite;

  Component *leaf_1 = new Leaf;
  Component *leaf_2 = new Leaf;
  Component *leaf_3 = new Leaf;
  branch1->Add(leaf_1);
  branch1->Add(leaf_2);
  Component *branch2 = new Composite;
  branch2->Add(leaf_3);
  tree->Add(branch1);
  tree->Add(branch2);
  std::cout << "Client: Now I've got a composite tree:\n";
  ClientCode(tree);
  std::cout << "\n\n";

  std::cout << "Client: I don't need to check the components classes even when managing the tree:\n";
  ClientCode2(tree, simple);
  std::cout << "\n";

  delete simple;
  delete tree;
  delete branch1;
  delete branch2;
  delete leaf_1;
  delete leaf_2;
  delete leaf_3;

  return 0;
}
```

