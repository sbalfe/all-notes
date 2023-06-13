# Getting started: Compiling and Running Java

- `javac file.java` $\to$ to **compile** 
- `java program-name` $\to$ **run** 
- **Simple Programs** $\to$ run `java file.java` , with **no classes** or shit to depend on.
- `ClassPath` $\to$ path to **where** java **searches for classes**.

- `jshell` - Interactive command line **program**.

```java
$ jshell
|  Welcome to JShell -- Version 11.0.2
|  For an introduction type: /help intro

jshell> "Hello"
$1 ==> "Hello"

jshell> System.out.println("Hello");
Hello

jshell> System.out.println($1)
Hello

jshell> "Hello" + sqrt(57)
|  Error:
|  cannot find symbol
|    symbol:   method sqrt(int)
|  "Hello" + sqrt(57)
|            ^--^

jshell> "Hello" + Math.sqrt(57)
$2 ==> "Hello7.54983443527075"

jshell> String.format("Hello %6.3f", Math.sqrt(57)
   ...> )
$3 ==> "Hello  7.550"

jshell> String x = Math.sqrt(22/7) + " " + Math.PI +
   ...> " and the end."
x ==> "1.7320508075688772 3.141592653589793 and the end."

jshell>
```

- The value of an **expression is printed** **without** needing to call `System.out.println` every time, but you can call it if you like.
- Values that are **not assigned to a variable** get assigned **synthetic identifiers**, like `$1`, that can be used in **subsequent statements.**
- The **semicolon** at the end of a statement is **optional** (unless you type **more than one statement on a line**).
- If you make a **mistake**, you get a **helpful message immediately**.
- You can get **completion** with a **single tab**, as in **shell filename completion.**
- You can get the relevant portion of the **Javadoc documentation** on **known classes** or **methods** with just a double tab.
- If you **omit a close quote**, **parenthesis**, or **other punctuation**, JShell will just **wait for you,** giving a **continuation prompt** (`…`).
- If you do make a **mistake**, you can use **“shell history”** (i.e., up arrow) to **recall** the **statement** so you **can repair it.**

---

- **Jshell** used for **prototpying** 

```java
$ jshell
|  Welcome to JShell -- Version 11.0.2
|  For an introduction type: /help intro

jshell> while (true) { sleep (30*60); JOptionPane.showMessageDialog(null,
  "Move it"); }
|  Error:
|  cannot find symbol
|    symbol:   method sleep(int)
|  while (true) { sleep (30*60); JOptionPane.showMessageDialog(null, "Move it");}
|                 ^---^
|  Error:
|  cannot find symbol
|    symbol:   variable JOptionPane
|  while (true) { sleep (30*60); JOptionPane.showMessageDialog(null, "Move it");}
|                                ^---------^

jshell> import javax.swing.*;

jshell> while (true) { Thread.sleep (30*60); JOptionPane.showMessageDialog(null,
"Move it"); }

jshell> while (true) { Thread.sleep (30*60 * 1000);
  JOptionPane.showMessageDialog(null, "Move it"); }

jshell> ^D
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210911140501826.png)

- Put **final working version** to `MoveTimer.java` 
  - Place a `class` statement and `main()` around **main line**
    - Told the **IDE** to *reformat* the **whole thing** $\to$ `darwinsys-api` **repository**.

## Using ClassPath effectively.

- Keep **classes** in some **common directory** otherwise use `classpath`. 
  - A path to a **list** of **directories** and/or **.jar** files.
    - That contain **classes** you want.
- `java HelloWorld` $\to$ the **java interpreter**  looks in **each places** named `CLASSPATH` for a **match**.

---

- `CLASSPATH` can be **set as an environment variable** the same as **any other**.
- Preferred to **specify** the `CLASSPATH` for some **given command** on the **command line**.

```bash
C:\> java -classpath c:\ian\classes MyProg
```

- Example
  1. Set `CLASSPATH` to `C:\classes` 
  2. Just **compiled** a **source file** named `HelloWorld.java` with **no package statement** $\to$​​ `HelloWorld.class` in *default directory* (**current directory**)
- Do **not set this variable**, as its **unpredictable** sometimes what can be left there
  - using `-classpath` will **override** the `CLASSPATH`. 

---

## Java IDEs

- Ensure good code completion.
- Ensure good refactoring.

## Downloading

- Download latest archive of this books files > unpack > run maven.

# Automatic Dependencies, Compilation, Testing, and Deployment with apache maven.

- Maven - Distributed Dependency management system that give it rules for building application packages such as JAR,WAR and EAR files.
- Deploying to a variety of different targets
- older build tools are how whereas maven focuses on what

---

- Controlled by `pom.xml` - Project Object Model

  ```xml
  <project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                        http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
  
    <groupId>com.example</groupId>
    <artifactId>my-se-project</artifactId> ## project name
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
  
    <name>my-se-project</name>
    <url>http://com.example/</url>
  
    <properties>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
  
    <dependencies> # depends on JUnit 4.x framework for unit testing.
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.8.1</version>
        <scope>test</scope>
      </dependency>
    </dependencies>
  </project>
  ```

- Typing `mvn install` in directory of this $\to$​ it ensures it has a **copy** of the **given version** of **junit**.
  - Compiles everything, setting `CLASSPATH` and other options for the compiler.
    - Run any unit tests.
      - If pass $\to$ generate **jar file** for the **program**.
        - Install in personal maven repo so other projects can depend on it.

- Program **source** expected in `src/main/java` 
- Program **tests** expected in `src/test/java` 
- If a **web** expected in `src/main/webapp` 

---

- Generate automatically with `mvn archetype:generate` 

---

- A `POM` file can **redefine** any of the **standards goal**
  - Common maven goals (defined by default do something sensible), include the following.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210914152058005.png)

## Gradle

- Uses a **domain specific language** based on the **JVM** based and **java based scripting language** *groovy*.

```groovy
# Simple Gradle Build for the Java-based DataVis project
apply plugin: 'java'
# Set up mappings for Eclipse project too
apply plugin: 'eclipse'

# The version of Java to use
sourceCompatibility = 11
# The version of my project
version = '1.0.3'
# Configure JAR file packaging
jar {
    manifest {
        attributes 'Main-class': 'com.somedomainnamehere.data.DataVis',
        'Implementation-Version': version
    }
}

# optional feature: like -Dtesting=true but only when running tests ("test task")
test {
    systemProperties 'testing': 'true'
}
```

## JUnit

- Using **debugger** is **time consuming**
  - Finding a **bug** in **released code** is **worse**.
    - Tried and **tested** means of **testing code** for a **long time**.

- Applied to **individual classes** in OOP languages often.
- Alternative to **system / integration testing** where a **complete slice** or **entire app** is **tested**.

---

- Classes that are **not main programs** include a **main method** that tests out or at least exercises **functionality.**

---

- Junit is a **java centric methodology** for **providing test cases** 
  - very **simple / useful**
    - Just write some test class that has some methods called `@test` for annotations.
- Junit uses **introspection** to **locate the methods** and **find all the methods** and then **runs** for **you.**
- Extensions of Junit handle tasks as diverse as **load testing** and **testing enterprise components**
- All modern IDE provide **built in support**. 

```java
public class PersonTest {

    @Test
    public void testNameConcat() {
        Person p = new Person("Ian", "Darwin");
        String f = p.getFullName();
        assertEquals("Name concatenation", "Ian Darwin", f);
    }
}
```

- 

