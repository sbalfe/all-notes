# Packages

- Always lower case
- Unique
- Use internet domain name, reversed as a prefix for package name.
- Replaced invalid characters such as - in domain with underscore
- Domain name components starting with a number should instead start with an underscore_
- Switch.supplier.com -> com.supplier._switch
- 1world.com -> com._1world
- Experts-exchange.com -> com.experts_exchange

---

#### Example Package names

- java.lang
- java.io
- org.xml.sax.helpers
- com.timbuchulka.autoboxing.

# Class Names

- ProperCase
- Class names should be nouns, they represent things.
- Start with capital letter
- Each word in the name should also start with capital, LinkedList.

# Interface

- Consider what the objects implementing are going to  become
- List / Comparable / Serializable.



# Constants

- Uppercase
- seperate with underscore
- use final keyword

```java
static final int MAX_INT;
```

