# Curves

- Curve can be **drawn with a pen**
- Curve = *set of points* that the pen **traces** over some **internal of time**
- Pen may move in 2D or 3D (*space curve*)
- Imagine the pen moving in some **other kind of space**
- Mathematical definitions:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221061256599.png)

- Both mention some **interval range** $\to$ the *time* over which the **pen traces the curve**
  - First definition $\to$ curve is the **set of points** that the **pen traces** (*image*)
  - Second definition $\to$ curve is **mapping** between **time** and that set of points
- We use **first definition**.

---

- A curve is an **infinitely large set of points**
  - The **points** in some **curve** have the **property** that any point has **two neighbours**.
    - Except for **small number of points** that have **one** such as the **endpoints**
      - May be infinite such as a line or **closed** like a **loop**

---

- Consider curves as the **insides** of **things** 
  - Must address the **specification** of curves, clear for **circles / segments / elliptical arcs**
- General curves that **do not have some name** are **==free form curves==** 

## Specification of curves

### Implicit

- Define the **set of points** on a curve by giving a **procedure** to **test** if the **point is on the curve**
  - Often some implicit curve representation defined by **==implicit function==** of this form:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221062402746.png)

- So this curve is the **set of points** for which this **holds**
  - Note that the **implicit function** $f$ is a **scalar function** $\to$ returns a **real number**

### Parametric

- Provide a **==free parameter==** to the **set** of *points* on the curve

  - Provides an **index** to the **points to the curve**

- The **parametric form** of a *curve* is a **function** that *assigns positions* to *values* of the **free parameter**

  - Think of a curve as something you draw with a pen on piece of paper
    - The **free parameter** in this case is **time**, ranging over this interval of time we draw said curve from start to finish.

- The **==parametric function==**  of this curve tells us where the **pen** is at **any instant** in **time**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211221062718818.png)

- This is **vector valued** function
  - This example is  a **2D curve**

### Generative / Procedural 

- Provide procedures to generate points on the **curve** that **do not fall** into the **first two categories**
  - Examples of **generative curve descriptions** include **subdivisions** *schemes and fractals.*

---

- Often refer to the **representation only** as curves have many.
- Thus *implicit curve* means a curve **represented** by an **implicit** **function** or to the **implicit function** that is one of the **representations** of the curve.
- Using the term *polynomial curve* $\to$ mean the **curve** represented by a **polynomial**.

---

- Curves must have a **parametric representation**
  - Can have other **representations** such as a circle in $2D$ with the **center** at the **origin** and **radius** as $1$. 
    - Written in this **explicit form**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211222025936149.png)

- Also as **parametric**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211222025947341.png)

---

- Parametric not always the most simple form.
- There are **advantages / disadvantages** to all representations.
  - Parametric are **easier to draw** by sampling the **free parameter**
    - Most common in CG are **easier to work with**.
- Our main **focus** will be **on parametric representations** of **curves**.

---

## Parameterization and Reparameterization. 

- A *parametric curve* $\to$ given by some **specific parametric function** over some *particular interval*.
  - Has a given **function** that is a *mapping* from an *interval* of the **parameters**.
    - Convenient for this to be from $0$ to $1$. 

---

- When the **free parameter** changes over this **unit interval**
  - Denote it as $u$. 

---

- View the **parametric curve** to be *some line drawn with a pen* 
  - Consider $u=0$ as the **initial start time** and $u=1$ as the **end** when **finished writing**.

---

- Curve specified by a **function** that **maps time** to *positions*.
  - Specification of the **curve** is a **function** that answers 
    - Where is the **pen at time** $u$. 

---

- Define a **function** $f(t)$ that **specifies** a *curve* over **interval** $[a,b]$ 
  - We define a **new function** $f_2(u)$ to specify the **same curve** over the **unit interval**
    - Define:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211222031603627.png)

- Then:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211222031704714.png)

---

- These functions $f$ and $f_2$ represent the **same curve**
  - They provide **different ==*parameterizations*==** of the *curve* 
- The process of *creating* a **new parameterization** for some *curve* is called an **==reparameterization==** 
  -  The *mapping* from *old parameters* to new ($g$ above) is the **==reparameterization function==**.

---

- Defining a **curve** by some *parameterization* 
  - There *exist* *infinitely many others* as you can just **reparametrize** 
- Having many **parameterizations** of a curve is *useful*
  - Allows us to create **parameterizations** that are *convenient*

---

- May be **problematic** however
  - It becomes **harder** to *compare* two functions to see if they **represent the same curve**
- The **free parameter** adds some **invisible** and **possibly unknown element** to the **curve representation** 
  - After drawing $\to$ we are **not aware** at the **speed** of the **interval**
    - $u=0.5$ is halfway over **parameter space** 
    - May *not be halfway along the curve* if the **motion** starts slow and speeds near the end.

---

- Consider these **representations**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211222032715966.png)

- They are **the same curve** on the **unit interval** 
  - When $u$ is not $0$ or $1$ $f(u)$ refers to a **different point** depending on the **representation** of said *curve*.

---

- If you are **given** some **parameterization** of a *curve* 
  - Use this **directly** as the *specification of the curve* $\to$ alternatively develop a **more convenient parameterization** 
    - Often $\to$ the **natural parameterization** is created in a way **that is convenient** (*natural*) for specifying the curve.
      - We **dont know** how the **speed** *changes* along the curve.

---

- Knowing the **pen moves** at some **constant velocity**
  - The **values** of the **free parameter** have **more meaning** 
    - Halfway through parameter space is **halfway** along the curve.

---

- Rather than **measuring time** $\to$ the **parameter** can measure **length** along the curve
  - These *parameterizations* are called **==arc-length parameterizations==**
    - Define **curves** by functions that **map** from the **distance** along the curve (*arc length*) to **positions**
      - Use $s$ to **denote** an **arc length parameter**.

- The **parameterization** is an *arc length parameterization* if the **magnitude** of its **tangent** has constant magnitude.

  - (The *derivative* of the **parameterization** with the **respect** to the **parameter**) = tangent.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211222050210921.png)

- Computation of the **length** *along a curve* can be **tricky**
  - Generally $\to$ defined by the **integral** of the **magnitude** of the **derivative** (*intuitively, the magnitude of the derivative is the velocity*)
- Thus , given a **value** for the **parameter** $v$ 
  - You can **compute** $s$ (the **arc length distance** along the **curve** from the **point** $f(0)$ to the point $f(v)$) :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211222053627434.png)

- Where $f(t)$ is a **function** that defines the **curve** with a **natural parameterization**
  - Using the *arc length* *parameterization* requires the solution of the above equation for $t$ given $s$.

---

- Often $\to$ for curves we **examine**
  - This cannot be **done in closed form** (*simple*) manner and **must be done numerically**.
- Generally $\to$ use the **variable** $u$ to denote **free parameters** that *range* over the **unit interval** 
  - $s$ to denote **arc length free parameters**
  - $t$ to **represent parameters** that are **not one of the other two.**

## Piecewise parametric representations

- For **some curves**

  - Defining **a parametric function** that represents their **shape** is **easy.** 

    - Such as *lines / circles / ellipses* $\to$ all have **simple functions** that define the **points** they contain in **terms** of a **parameter**
  
- Often for curves

  - Finding some **function** that *specifies* the **shape** can be **hard**

---

- The **main strategy** we use to **create complex curves** is *divide and conquer*
  - Break curve into a **number** of **simpler** and **smaller pieces**
    - Each having some **simple description**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211222064431175.png)

- First specified in **two pieces**
  - $(b)$ requires a **circle** and a **line segment** this is **compound curve**
- Creating a **parametric representation** of this
  - Make our **parametric function** *switch* between the **functions** that **represent the pieces**
  - Range over $0 \leq u \leq 1$ we have:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211222064715262.png)

- $f_1$ parameterization of the **first piece**
- $f_2$ is the *parameterization* of the **second piece**
- Both defined over the **unit interval.**

---

- Ensure **continuity** 
  - If $f_1(1) \neq f_2(0)$ then the **pieces** will **not connect** and not **form some continuous curve.**

---

- We use a **line segment** and a **circular arc** 
  - We **can not precisely** recreate this with **just line segments** other than **using infinite ammount**.
    - It may be **feasible** still.
- We *prefer simplicity* of **line segment pieces** to having a **curve** that **more accurately represents the shape** for $(c)$ 

---

- Notice we use an **increasing** number of **pieces** and obtain a **better approximation**
  - Taking a **limit** we can **exactly represent the shape**.

---

- Advantage of a **piecewise representation** is we can **tradeoff** between:
  1. How well the **represented curve approximates** the *real shape* we are *representing*
