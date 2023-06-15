# Vector Spaces

- Linear algebra is just **linear maps studying** on *finite dimensional vector spaces*.

## $\R^n$ and $\C^n$

### Complex Numbers

- Complex numbers are made so we **can take square roots** of **negative numbers**.
  - Assume we have a $\sqrt{-1}$ denoted $i$ 
    -  Obeys the **following arithmetic**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130163750585.png)

- If $a \in \R$ $\to$ identify $a+0i$ with the **real number** $a$.
  - Therefore think of $\R$ as a **subset** of $\C$ 

---

- Easily verify that $i^2 = -1$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130164048070.png)

#### Properties of Complex Arithmetic

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130164137524.png)



---



![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130164249069.png)



---



#### Subtraction / Division in $\C$

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130164445696.png)

- Make **definitions** and *prove theorems* that apply to **both real / complex numbers**

#### $F$ notation

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130164602172.png)

- Used as they are both **fields** $\C$ and $\R$. 

---

- Proof of $F$ $\to$ must hold for $\R$ and $\C$ 
  - Elements of $F$ are **scalars** $\to$ the word **scalar**
    - A word that simply **means** *number* $\to$ often used when we **wish to emphasize** that an **object** is a **number** rather than a *vector*.

---

- For $\alpha \in F$ and $m$ a **positive integer**
  - Define $\alpha^m$ $\to$ denote the **product** of $\alpha$ with itself $m$ times 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130165229216.png)

### Lists

https://math.stackexchange.com/questions/946477/lists-versus-sets-in-linear-algebra

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130165307200.png)

- To **generalize** $\R^2$ and $\R^3$ to **higher dimensions**.
  - Consider the idea of *lists*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130165516828.png)

- Therefore a **list of length** $2$ is an **ordered pair** and $3$ an **ordered triple**.
- May use **list** without specifying **its length**
  - Recall that the **definition** each **list** has a **finite length** that is **nonnegative integer**
    - Therefore an **object looks like**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130170053140.png)

- Therefore **infinite length** $\to$ is *not a list*.
  - length $0$ list looks like $()$ 
    - Consider such an **object** to be a **list** so that some **of our theorems** will *not have trivial exceptions*
- For list next to **sets**
  - *order matters* and *repetitions have meaning*
    - These have **no meaning** in *sets* 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130170609806.png)

### $F^n$ 

- To define **higher dimensional** *analogues* of $\R^2$ and $\R^3$
  - Simply replace $\R$ with $F$ (*equivalent* to $\R$ and $\C$) 
    - Replace the $power$ with **arbitrary** *positive integer*
      - Fix some *positive integer* $n$ for the **rest of this section**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130170838930.png)

> --- self note --
>
> So its a set of all lists that contain n elements , where the elements are defined by j arbitrary value from 1 to n
>
> where $n$ is within what **number system** $F$ states.

- If $F=\R$ and $n$ equals $2$ or $3$
  - Then the **definition** of $F^n$ agrees with the **previous notations**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130171711900.png)

- We **cannot visualise** $n \geq 4$ as some **physical object**

- $\C^1$ can be **thought** of as a **plane**.

  - But for $n \geq 2$ $\to$ our **brain** can *not provide* a **full image** of $\C^n$ 

- Even if $n$ is **large** we can **perform algebraic manipulations** in $F^n$ as easily in $\R^2$ or $\R^3$ 

  - Addition in $F^n$ is **defined** as *follows*

    

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130172355755.png)

- Becomes *cleaner* using a **single letter** to *denote* the *list* of $n$ numbers
  - Without **explicitly writing the coordinates**.
  
- Result below stated with $x$ and $y$ in $F^n$ 
  - Even though the **proof** requires the **more cumbersome** $(x_1, \cdots, x_n)$ and $(y_1, \cdots , y_n)$.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130172630012.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130172641495.png)

---

- The **second / fourth** *equalities* hold because of the **definition** of **addition** in $F^n$ 
- **third equality** holds via **commutativity** of addition within $F$.

---

- If a **single letter** then $x \in F^n$ , letting $x = (x_1, \cdots, x_n)$ is **good notation**
  - Working with $x$ to **avoid explicit coordinates** *also*.

#### 0 Definition

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130173212150.png)

- Using $0$ in **two different ways**
  - On the **left hand side** of the *equation* in $1.14$ 
    - The symbol $0$ denotes a **list** of **length** $n$ whereas the **right side** each $0$ denotes a **number**.
- The **context** makes it **clear** what is **intended**.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130173438756.png)

---

- A picture to aid **visualisation** of $\R^2$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130173546711.png)

- Think of $(x_1,x_2)$ as an **arrow** rather than **a point**, thus a **vector**.
  - Moving one **parallel** to *another* $\to$ it becomes the **same vector**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130173726392.png)

- Points / Vectors are just **aids to our understanding**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130173930279.png)

- Example $(2, - 3, 17, \pi , \sqrt{2})$ is an **element** of $\R^5$ 
  - May **casually** refer to it as a **point** within $\R^5$ or even a **vector**
    - With **no consideration** of the **geometrical interpretation** 

---

- We have observed **addition** in $F^n$ yields an *element* that is **within** $F^n$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130174224404.png)

- Obtain from **adding** the *corresponding coordinates* 
  - From above **addition** has a **simple geometric interpretation** within $\R^2$. 
- Say you have two vectors $x$ and $y$ in $\R^2$ we wish to **add**
  - Move the vector $y$ **parallel** to itself that its **initial point** *coincides* with the **end** of vector $x$ 
    - The sum $x+y$ is the **vector** whose **initial point** is $x$ **initial point** and whose **end** is $y$ **end point**.

---

- For this definition $0 \in F^n$ , $(0,0,0, \cdots, 0)$ 

#### Additive Inverse

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130174657890.png)

- For $\R^2$ the **==additive inverse==** $-x$ is the vector **parallel** to $x$ in the **opposite direction** 
  - Figure here **illustrates** this way of **considering** the **additive** inverse in $\R^2$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130174939080.png)

- Can adapt **addition** to **multiplication** in $F^n$ 
  - Start with **two elements** of $F^n$ and obtain **another element** of $F^n$ by **multiplying** the *corresponding coordinates*
- This **definition** is *not useful* for **our purposes**.
  - Alternative **multiplication** is **==scalar multiplication==**.
- Must define what it **means** to *multiply* elements of $F^n$ by an element in $F$ 

#### Scalar multiplication in $F^n$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130175323321.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130175444624.png)

- If $\lambda$ is a **positive number** and $x$ is a **vector** in $\R^2$ 
  - Then $\lambda x$ is the **vector** that **points** in the **same direction** as $x$ and whose length is $\lambda$ times the **length** of $x$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130175759378.png)

- To obtain $\lambda x$ $\to$ must **stretch** or **shrink** $x$ by some factor $\lambda$ 
  - Depending on $\lambda < 1$ or $\lambda > 1$.

- If $\lambda$ is **negative** and $x$ is a vector in $\R^2$ 
  - Then $\lambda x$ is the **vector** that **points** within the **opposite direction** to $x$ 
    - And whose length is $| \lambda |$ times the **length** of $x$ 

### Digression on Fields

- A **==*field*==** is a **set** that contains at **least two distinct elements** called $0$ and $1$ 
  - Along with the **operations** of
    1. Addition
    2. Multiplication
  - Satisfying properties of $1.3$ (*properties of complex arithmetic*) 
- Therefore $\R$ and $\C$ are **fields**
  - So is the set of **rational numbers** along with the **usual** operations of **addition / multiplication**
- Alternative example is the set $\{0,1\}$ with the **usual operations** of **addition / multiplication** 
  - Except that $1+1$ defined to **equal** $0$. 

- Often definitions with $\R$ and $\C$ fields extend to **arbitrary fields**
  - Think of $F$ as denoting an **arbitrary field** instead of $\R$ or $\C$ 

## Definition of Vector Space

- The **idea** of **vector spaces** comes from **properties** of **addition / multiplication** in $F^n$ 
  
- Every *element* has some **additive inverse**
  
  1. Addition is **commutative**
  2. Addition is **associative**
  3. Addition has an **identity**
  
  ---
  
  1. **Scalar multiplication** is **associative**.
  
  2. **Scalar multiplication** by $1$ acts **as expected**.
  
  3. **Addition / Scalar multiplication** are *connected* by **distributive properties**. 

---

- Define a **vector space** to be a set $V$ with an **addition** and a **scalar multiplication** on $V$
  - That **satisfies** the *properties* above.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130181928651.png)

- Formal vector space definition:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130182106282.png)

- Geometric language **aids our intuition**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130182408500.png)

- The **scalar multiplication** in a **vector space** depends on $F$ 
  - Therefore we must be **precise** 
    - Say that $V$ is a **vector space over** $F$ rather than saying $V$ is a **vector space**. 

- Example $\to$ $\R^n$ is a **vector space** over $\R$ 
- $\C^n$ is a **vector space** over $\C$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130183027117.png)

- The choice of $F$ is either **obvious** from the **context** or **irrelevant** 
  - Assume its in the background
- With the standard operations of **addition / scalar multiplication**
  - $F^n$ is a **vector space** over $F$  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130183257481.png)

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130183428852.png)

- This example of *vector space* involves a **set of functions**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130183952350.png)

> -- self notes --
>
> so its a vector space of functions defined in the domain S to the codomain F
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130191518497.png)

- As **an example** of the **notation above** 
  - If $S$ is the **interval** $[0,1]$ and $F= \R$  
    - Then $R^{[0,1]}$ is the **set** of **real valued functions** on the *interval* $[0,1]$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130184406651.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130191135196.png)

- Our **previous examples** of *vector spaces* 
  - $F^n$ and $F^{\infty}$ are **special cases** of the **vector space** $F^S$ as a **list** of length $n$ of **numbers** in $F$ can be.
    - Thought of as a **function** from $\{1,2, \cdots,n\}$ $\to$ $F$.
    - And a **sequence of numbers** in $F$ thought of as a **function** from the **set of positive integers** to $F$ 
  - In other words, think of $F^n$ as $F^{\{1,2,\cdots, n\}}$ and we **can think** of $F^{\infty}$ as $F^{\{1,2, \cdots\}}$  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130194627513.png)

> -- self notes --
>
> Consider the $\R^2$ , its $\{1,2\} \to \R$ , equivalent to $(a_1,a_2) \in \R^n$ 
>
> Considering **2 element** indices that map to **pairs** of values in $\R$.   

---

- Soon $\to$ see **further examples** of **vector spaces**
  - The **definition** of a **vector space** *requires* that it have an **==additive identity==** 
    - Result below **states** that this **identity is unique**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130200018406.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130200032291.png)

- Each element $v$ in a **vector space** has an **additive inverse**
  - An element $w$ in the **vector space** such that $v+w = 0$ 
    - The next result **shows** that **each element** in a **vector space** only has **one additive inverse**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130200611049.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130200620583.png)

- Because **additive inverses** are **unique**
  - This notation **now makes sense**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130200752058.png)

- Nearly **every result** in this book involves some **vector space**.
  - Add shorthand notation for $V$:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130201232152.png)

- 0 is **scalar** on **LHS** ($0 \in F$) 
- 0 is a **vector** on **RHS** (the **additive identity** of $V$) 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130201605314.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130201701926.png)

- For this result below
  - $0$ $\to$ denotes the **additive identity** of $V$ 
    - Similar proof this is **not the same** as $1.29$ 
      - $1.29$ states the **product** of scalar $0$ and any **vector** equals **vector** $0$ 
      - $1.30$ states the **product** of any *scalar* and the **vector** $0$ equals the **vector** $0$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130202225796.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130202236853.png)

> -- self notes --
>
> in both proofs, they add the additive inverse to the axiom denoted in the second line of the proof
>
> saying that 0 + 0 = 0 which is clearly known to be true.

- We now show that **if an element** of $V$ is **multiplied** by the **scalar** of $-1$ 
  - Then the **result** is the **additive inverse** of the element $V$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202234457272.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211202234512302.png)

## Subspaces



![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203003359564.png)

- Consideration of **subspaces** expands the **examples** of **vector spaces**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203002029355.png)

> -- self notes --
>
> So its like as it says, a subsection of the vector space following the same addition / scalar multiplication as defined on V

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203003344325.png)

- Next result gives the **fastest method** to check whether a **subset** of a **vector space** is a **subspace**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203003604199.png)

### Proof (conditions of a subspace)

- If $U$ is a **subspace** of $V$ $\to$ $U$ **satisfies** the three conditions above via the **definition** of a **vector space*

---

- *Conversely* , suppose $U$ **satisfies** the **three conditions above** 
  - The *first condition* ensures the **additive identity** of $V$ is *in* $U$. 
  - The *second condition* ensures that **addition** makes sense on $U$.
  - The *third condition* ensures that **scalar multiplication** makes sense on $U$ 
- If $u \in U$ $\to$ $-u$ (equivalent to $(-1)u$ `1.31`) $\to$ is **also** in $U$  by the **third condition above**
  - Thus every element of $U$ has an **additive inverse** in $U$ 
- The other sections of **a vector space**
  1. Commutativity
  2. Associativity
- Are **automatically satisfied** for $U$ because they **hold** on the **larger space** $V$ 
  - Therefore $U$ is a **vector space** and thus a **subspace** of $V$.

---

- The **three conditions** in the *result* above often **enable** us to **determine** quickly whether **a given subset** of $V$ is a **subspace** of $V$. 

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203005726676.png)

---

- Verification of **these items** above shows the **linear structure** underlying parts of **calculus**
  - Section above requires the **result** that the **sum** of **two continuous functions** is **continuous**
- Another *example* $\to$ the **fourth item** requires the **result** that for some **constant** $c$ 
  - The *derivative* of $cf = c$ *times* the *derivative* of $f$ 

---

- The *subspaces* of $\R^2$  are **precisely**:
  1. $\{0\}$ 
  2. $\R^2$ 
  3. All lines in $\R^2$ through the **origin**
- The *subspaces* of $\R^3$ are **precisely**:
  1. $\{0\}$
  2. $\R^{3}$
  3. All lines in $\R^3$ through the **origin** 
- To **prove** that all these *objects* are **subspaces** is *clear*
  - Hard idea is showing they **only** the **subspaces** of $\R^2$ and $\R^3$ 

### Sums of Subspaces

- When **dealing** with **vector spaces**
  - Often *interested* only in **subspaces** next to **arbitrary subsets**
- The **sum of spaces** idea is **useful**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203011631344.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203011638912.png)

- Examples of *sums of subspaces*

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203011751206.png)

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203011958050.png)

---

- The **next result** $\to$ states that the **sum of subspaces** is a **subspace**
  - And is in fact the *smallest subspace* containing **all the summands**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203013721672.png)

#### Proof (sums of subspace is the smallest containing subspace)

- Easy to see that $0 \in U_1 + \cdots + U_m$ and that $U_1 + \cdots + U_m$ is **closed under addition** and **scalar multiplication** 
  - Thus `1.34` *implies* that $U_1 + \cdots + U_m$ is a **subspace** of $V$ 
- Clearly $U_1 , \cdots, U_m$ are **all contained** in $U_1 + \cdots + U_m$ 
  - *consider* the **sums** $u_1, \cdots, u_m$ where all except **one** of the **u's** are $0$.
- Conversely, every **subspace** of $V$ containing $U_1, \cdots, U_m$ contains  $U_1 + \cdots + U_m$ 
  - *because* **subspaces** must contain all **finite sums of elements**.
- Therefore $U_1 + \cdots + U_m$ is the **smallest subspace** of $V$ containing $U_1, \cdots, U_m$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203020738447.png)

### Direct Sums

https://www.statlect.com/matrix-algebra/direct-sum

- Suppose $U_1, \cdots , U_m$ are **subspaces** of $V$ 
  - Every *element* of $U_1 + \cdots + U_m$ can be **written** in the **form**:

$$
\large u_1 + \cdots +u_m
$$

- Where each $u_j$ is *in* $U_j$ 
  - Notably , the *cases* where each vector in $U_1 + \cdots + U_m$ can be **represented** in the **form** above in a **single way**
- This is a **vital attribute** and is called the **==direct sum==**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203021307205.png)



![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203021853349.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203022428438.png)

---

- Sometimes **non examples** add to **our understanding**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203022649883.png)

> **Solution**
>
> - $F^3 = U_1 + U_2 + U_3$  as every **vector** $(x,y,z) \in F^3$ may be **written**:
>
> $$
> \large (x,y,z) = (x,y,0) + (0,0,z) + (0,0,0)
> $$
>
> - Where the **first** vector on the **right side** is **in** $U_1$ , **second** , **third** in $U_2, U_3$ respectively.
> - $F^3$ however does **not equal** the **direct sum** of $U_1,U_2,U_3$
>   - Because the vector $(0,0,0)$ may be written in **two different ways** as a sum $u_1 + u_2 + u_3$ 
>     - With each $u_j$ in $U_j$ , Specifically we have:
>
> $$
> \large (0,0,0) = (0,1,0) + (0,0,1) + (0,-1,-1)
> $$
>
> - And
>
> $$
> \large (0,0,0) = (0,0,0)+ (0,0,0) + (0,0,0)
> $$
>
> - Where the **first vector** on the **right side** of *each equation* above is **in** $U_1$ , **second / third** in $U_2,U_3$ *respectively*.
> - The **definition** of *direct sum* requires that **every vector** in the **sum** have a **==unique representation==** as an **appropriate sum**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203023526547.png)

- The **next result** shows when **deciding** whether a **sum** of **subspaces** is a *direct sum*
  - We need only **consider** whether $0$ can be **uniquely written** as an **appropriate sum**

#### Condition for a direct sum

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203024155074.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203024459023.png)

> -- self notes --
>
> The proof takes two different vector values from the direct sum of the subspaces
>
> just using v and u to differentiate then takes them negated off each other and shows this is equivalent 0
>
> implying that they are all the same for the entirety of the entire direct sum which is exactly 

- Next results gives a **simple condition** for *testing* which **pairs of subspaces** give a **direct sum**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203025649472.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203030122111.png)

> -- self notes --
>
> if u = -w for the last part this implies that its also in W as -w is just an additive inverse which is defined in the subspace
>
> and thus as u is contained in both U and W i.e. the intersection it must be 0, thus -w = 0, thus w = 0 too, proof.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203030650028.png)

- The **result** above deals with **only** the case of **two subspaces**
  - When asking about a **possible direct sum** with $>2$ **subspaces**
    - Its not enough to **test** that *each pair* of the **subspaces** is $0$ 
- To see this , consider example `1.43` 
  - In that **nonexample** of a **direct sum** we have:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203031214758.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203032816278.png)

> -- self notes --
>
> showing a quick closure under addition / scalar multiplication example.
>
> idea is we apply addition and if it holds the predicate its closed
>
> same idea for multiplication.

