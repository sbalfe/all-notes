# Environment Interaction With Java

- `System` knows lots about our **system**
- `java.lang.Runtime` is behind the methods of `System` 
- `System.exit()` $\to$ `Runtime.exit()`
- `Runtime` is part of the environment , but only time we use it directly is to **run other programs**

---

## Environment variable

- Use `System.getenv()` to obtain the **environment variables**.

---

- Use these variables for **specific values** on **different machines**
- `PATH` determines where the **system** looks for **executable programs**.
- Some OS may **not provide** a certain variable, so be **cautious**.

```java
public class GetEnv {
    public static void main(String[] argv) {
        System.out.println("System.getenv(\"PATH\") = " + 		    		System.getenv("PATH"));
    }
}
```

```java
C:\javasrc>java environ.GetEnv
System.getenv("PATH") = C:\windows\bin;c:\jdk1.8\bin;c:\documents
    and settings\ian\bin
C:\javasrc>
```

---

- `System.getenv()` returns **all variables** with **no argument**
- Both require **permissions to use of course**.

## System Properties

- `System.getProperty` or `System.getProperties()` to obtain information on system properties.

---

- A **Property** $\to$​​ a **name : value** pair in `java.util.properties` 
- The `System.Properties` object controls and describes the Java runtime. The `System` class has a static `Properties` member whose content is the merger of operating system specifics (`os.name`, for example), system and user tailoring (`java.class.path`), and properties defined on the command line (as we’ll see in a moment). Note that the use of periods in these names (like `os.arch`, `os.version`, `java.class.path`, and `java.lang.version`) makes it look as though there is a hierarchical relationship similar to that for package/class names. The `Properties` class, however, imposes no such relationships: each key is just a string, and dots are not special.

---

- View all **defined system properties**
  - You can **iterate through** the **output**

```java
jshell> System.getProperties().forEach((k,v) -> System.out.println(k + "->" +v))
awt.toolkit->sun.awt.X11.XToolkit
java.specification.version->11
sun.cpu.isalist->
sun.jnu.encoding->UTF-8
java.class.path->.
java.vm.vendor->Oracle Corporation
sun.arch.data.model->64
java.vendor.url->http://java.oracle.com/
user.timezone->
os.name->OpenBSD
java.vm.specification.version->11
... many more ...
jshell>
```

- Retrieve specific value of **property**

---

- Give java **environment variables on compilation**
  - Use `-D` 

```bash
java -D"pencil_color=Deep Sea Green" environ.SysPropDemo
```

- Can be set in maven and gradle.
