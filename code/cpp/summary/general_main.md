# C++ Terms

### [Basic concepts](https://en.cppreference.com/w/cpp/language/basic_concepts)

### **Declarations**

- Can introduce **entities**, associate them with **names** and define their properties. Declarations that declare all properties required to use an entity are **definitions**.

### **Definitions**

- Definitions are declarations that fully define the entity introduced by the declaration.
- Function definitions include sequences of **statements**, some of which include **expressions** that specify the computations to be performed by the program.

### **Translation**

### **main function**

### **keywords**

### **Identifiers**

> https://en.cppreference.com/w/cpp/language/identifiers

- An *identifier* is an arbitrarily long sequence of digits, underscores, lowercase and uppercase Latin letters, and most Unicode characters.

### **Comments**

- Comments serve as a sort of in-code documentation. When inserted into a  program, they are effectively ignored by the compiler; they are solely  intended to be used as notes by the humans that read source code.  Although specific documentation is not part of the C++ standard, several utilities exist that parse comments with different documentation  formats

### **literals**

- Literals are the tokens of a C++ program that represent constant values embedded in the source code.

### **escape sequences**

- Used to represent certain special characters within **string literals** and **character literals**.

### **Entities**

- In a C++ program, the entities are **values**, **objects**, **references**, **structured bindings**, **functions**, **enumerators**, **types**, class members, **templates**, **template specializations**, **parameter packs**, and **namespaces**. Pre-processor **macros** are not entities.

### **Values**

### **Objects**

### **References**

### **Structured bindings**

### **functions**

### **enumerators**

### **types**

- **Objects**, **references**, **functions** including **function template specializations**, and **expressions** have a property called type, which both restricts the operations that are permitted for those entities and provides semantic meaning to the otherwise generic sequences of bits.

### **class members**

### **templates**

### **template specializations**

### **parameter packs**

### **namespaces**

### **macros**

- The preprocessor supports text macro replacement. Function-like text macro replacement is also supported.

### **names**

- A name is the use of one of the following to refer to an entity - 

  Names

  - Each name is only valid within a part of the program called its **scope**.
  - Some names have **linkage** which makes them refer to the same **entities** when they appear in different scopes or translation units.

### [**name lookup**](https://en.cppreference.com/w/cpp/language/lookup)

- Names encountered in a program are associated with the declarations that introduced them using name lookup.
- [**Unqualified name lookup**](https://en.cppreference.com/w/cpp/language/unqualified_lookup)

### **scope**

> https://en.cppreference.com/w/cpp/language/scope

### **linkage**

- A name that denotes object, reference, function, type, template, namespace, or value, may have *linkage*. If a name has linkage, it refers to the same entity as the same name introduced by a declaration in another scope. If a variable, function, or another entity with the same name is declared in several scopes, but does not have sufficient linkage, then several instances of the entity are generated.