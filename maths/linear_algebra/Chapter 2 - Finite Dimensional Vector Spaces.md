Finite Dimensional Vector Spaces

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204001702235.png)

---

## Span And Linear Independence

- So far $\to$ written **lists** of **numbers** surrounded by **parentheses**
  - Continue this trend for *elements* of $F^{n}$ 
    - Example $\to$ $(2,-7,8) \in F^3$ 
- Must consider **lists** of **vectors**
  - May be **elements** of $F^{n}$ or **other vector spaces**.
- Often write *lists of vectors* with **no surrounding parentheses**
  - Example:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204001950620.png)

- Is a **list** of *length* $2$ of **vectors** within $\R^3$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204002019073.png)

### Linear combinations and Span

- Adding the **scalar multiples** of **vectors** in a *list* $\to$ gives us a **linear combination** of the **list**
  - The **formal definition shown below**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204002135171.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204002147836.png)

> -- self notes --
>
> solving gets the bottom two intersecting however the top value does not therefore there are no solutions.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204003622647.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204003757553.png)

> -- self notes --
>
> the first vector is in the span as it can be made from a multiple of the vectors in 
>
> where the scalar applies is in $F^3$ , this does not hold for the vector below.

- May be referred to as the **==linear span==** which is *equivalent* to the idea of **span**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204003946022.png)

#### Proof (Span is the smallest containing subspace)

- Suppose $v_1, \cdots, v_m$ is a **list of vectors** in $V$ 
- First show that the the $span(v_1, \cdots, v_m)$ is a **subspace** of $V$ 
- The **additive identity** is *in* the $span(v_1, \cdots, v_m)$, because:

$$
\large 0 = 0v_1 + \cdots + 0v_m
$$

- *Also* $\to$ the $span(v_1, \cdots, v_m)$ is **closed under addition**, because:

$$
(a_1v_1 + \cdots + a_mv_m) + (c_1v_1 + \cdots + c_mv_m) = (a_1+c_1)v_1+\cdots+(a_m+c_m)v_m
$$

- *Furthermore* $\to$ $span(v_1, \cdots, v_m)$ is **closed under scalar multiplication**, because:

$$
\lambda(a_1v_1 +\cdots+a_mv_m) = \lambda a_1v_1 + \cdots + \lambda a_mv_m
$$

- *Thus* $\to$ $span(v_1, \cdots, v_m)$ is a **subspace** of $V$ 

  - Each $v_j$ is some **linear combination** of $v_1, \cdots, v_m$ (*set* $a_j = 1$ and let the other a's in `2.3` $=0$)

  > -- self notes -- this obtains the $v_j$ as all others are 0 thus leaving just $v_j$ thus linear combination

  - *Therefore* $\to$ $span(v_1, \cdots, v_m)$ contains *each* $v_j$.

- *Conversely*

  - Because **subspaces** are *closed under scalar multiplication* and *addition*
  - Every **subspace** of $V$ containing each $v_j$ contains $span(v_1, \cdots, v_m)$ 

- *Therefore* $\to$ $span(v_1, \cdots, v_m)$ is the **smallest subspace** of $V$ containing **all the vectors** $v_1, \cdots, v_m$.

---

> -- self notes --
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204005901487.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204010019650.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204010030824.png)

> -- self  notes --
>
> so in $F^n$ they took its vectors and applied them to the $n-tuple$ to obtain a complete **linear combination** of each tuple and thus the n tuple is said to have **spanned** over $V$ or its **spans** $V$. 

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204010423431.png)

- Recall that **every list** has a **finite length**
  - Example `2.9` above shows that $F^n$ is a **finite dimensional vector space** for *every positive integer* $n$ 

### Polynomials

- The **polynomial definition** is *already familiar surely* (or you may be a retard)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204010701610.png)

- This is combined with operations of **addition / scalar multiplication**

  - $\mathcal{P}(F)$  is a **vector space** *over* $F$ as you **should verify** (*scalar multiplication / addition / additive identity*) 
- Alternatively, $\mathcal{P}(F)$ is a **subspace** of $F^{F}$ 
  - The **vector space** of *functions* from $F \to F$. 
- If some **polynomial** (*thought* of as a function from $F \to F$) 
  - Is represented by **two sets of coefficients** 
    - Then *subtracting* one *representation* of the **polynomial** from the **other** $\to$ produces a **polynomial** that is **identically zero** as a *function* on $F$ and thus has **all zero coefficients**
- *Conclusion*
  - The **coefficients** of a *polynomial* are *uniquely determined* by the **polynomial**
    -  Thus the **next definition** *uniquely* defines the **degree of a polynomial**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204013011916.png)

- For the **next definition**
  - We use the **convention** that $-\infty < m$ 
    - This means the **polynomial** $0$ is *in* $\mathcal{P}_m{F}$  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204022152202.png)

- To **verify** the **next example** $\to$ note $\mathcal{P}_m(F) = span(1, z, \cdots, z^m)$ 
  - Letting $z^k$ denote **some function**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204022453400.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204022710669.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204022833378.png)

### Linear independence

- Suppose $v_1, \cdots, v_m \in V$ and $v \in span(v_1, \cdots, v_m)$ 
  - Via the **spans definition** $\exists \ a_1, \cdots, a_m \in F$ *such that*: 

$$
\large v= a_1v_1 + \cdots + a_mv_m
$$

- Consider whether the **choice** of such **scalars** above is *unique*
  - Suppose $c_1, \cdots, c_m$ is **another set** of scalars **such that**:

$$
\large v=c_1v_1 + \cdots + c_mv_m
$$

- *Subtracting* the **last two equations**:

$$
0  = (a_1-c_1)v_1 + \cdots + (a_m-c_m)v_m
$$

- Thus writing $0$ as a **linear combination** of $(v_1, \cdots , v_m)$ 
  - If the **only way** to do this is the **obvious method** ($0$ for **all scalars**)
    - Then each $a_j - c_j = 0$ and **therefore** $a_j = c_j $ (Thus the **scalars** are **unique**)
- The **situation** is vital and is called **==linear independence==**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204024400876.png)

- This **reasoning above** shows that $v_1, \cdots, v_m$ is *linearly independent* 
  - **iff** each *vector* in $span(v_1, \cdots, v_m)$ has a **single representation** as a *linear combination* of $v_1, \cdots, v_m$. 

> -- self notes --
>
> single representation is where that vector is 1 and the others are 0, there are no other linear combinations
>
> in the span of the vector which can be equivalent to that vector thus they are **linearly independent** 
>
> below is a discussion of methods to show linear independence
>
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204031246214.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204024824506.png)

- If some vectors are **removed** from a **linearly independent** *list*
  - The **remaining list** will also be **linearly independent** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204025330908.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204025401487.png)

- This **lemma** is **often useful**
  - States that given a **linearly dependent** *list of vectors*
    - One of the vectors is **in the span** of the **previous ones** 
      - *Furthermore* $\to$ we can **throw** out the **vector** *without changing* the *span* of the **original list**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204032028299.png)

#### Proof (Linear Dependence Lemma)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204040332213.png)

> -- self notes --
>
> part 1 just rearranges the $v_j$ to obtain a span of the vectors from $v_1$ to $v_{j-1}$ 
>
> this is of course taking that $a_j$ $\neq 0$ 
>
> second part just uses the list above and shows that some vector still spans the list of the original vectors even if the 
>
> vector was removed.

- Selecting $j = 1$ in the **lemma above**
  - Means that $v_1 = 0$ as $j=1$ would say that $(a)$ is interpreted as $v_1 \in span() = \{0\} $ 
- The **proof** of $(b)$ must be **modified** in some **obvious way** if $v_1 = 0$ and $j=1$ 
  - $u = c_1v_1 + \cdots + c_mv_m = c_2v_2 + \cdots + c_mv_m$ 
- Generally the **other proofs** of this book will **not call for special cases** that must be *considered* for involving
  1. Empty Lists
  2. Length 1 List
  3. The subspace $\{0\}$
  4. Any other **trivial case** for which the result is true but **require different cases** for the **proof to hold**.

- It says that **no linearly independent** list in $V$ is *longer* than a **spanning list** in $V$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206042005754.png)

#### Proof (Length of linearly independent list <= length of spanning list)

- Suppose $u_1, \cdots, u_m$ is **linearly independent** in $V$ 
- Suppose also $w_1, \cdots, w_n$ *spans* $V$ 
- Must prove that $m \leq n$ 
- Perform a **multi step process** described below
- Within each step we **add one** of the $u's$ and remove one of $w's$ 

##### Step 1 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206042352402.png)

##### Step j

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206042725317.png)

- After step $m$ we have added all the $u's$ and the **process stops**
- At *each step* $\to$ as we add a $u$ to $B$ $\to$ the **linear dependence lemma** *implies* that there is some $w$ to remove
- Thus there at **least** as many $w's$ as $u's$. 

> -- self notes --
>
> - So if B $w_1, \cdots, w_n$ *spans* $V$ then $span(B) = V$ 
> - Adding some vector to this list that was $V$ produces some **linearly dependent list** (as this vector was in $V$ , and B spans V then it must be a **linear combination** of the **other vectors**)
> - So there is some list $u_1, w_1, \cdots , w_n$ which is **linearly dependent** 
> - Removing a $w$ to make new List length $n$ with $u_1$ and the other w's still spanning V
>
> ---
>
> - Second part B from previous step $j-1$ spans V as stated
> - Adjoining any vector to this list produces a **linear dependent list** 
> - The list of length $(n+1)$ this creates by adjoining some $u_j$ to $B$ , placing after $u_1, \cdots, u_{j-1}$ is **linearly dependent **
> - Via the lemma one of the vectors in the list is in the **span** of the other one and as $u_1, \cdots, u_j$ is **linearly independent** (changing the number of $u$ does not **affect this property**), this vector must be a $w$ and not a $u$ 
> - We can remove this $w$ from $B$ now length $n$ containing $u_1, \cdots, u_j$ and the remaining $w's$ span $V$ 
>
> ---
>
> - Conclusion, after step $m$ we added all the $u's$ and the process ends
> - each step as you add $u$ to $B$, the lemme implies there is $w$ to remove therefore there must be at **least** as many **w's** as **u's**
>
> https://math.stackexchange.com/questions/2262094/prove-that-length-of-linearly-independent-list-leq-length-of-spanning-list - explains why we have to remove w from the first step and not u, basically because the u cannot be in the span of everything on its left as its a **linearly independent list** and thus forms a **contradictive illogical state**.
>
> Summary: we have a list of $u_1, \cdots, u_j,w_1, \cdots, w_k$ and adding another $u$ means we **must remove** $w$ to make the length always <= , add u must remove w.

- Next two examples show how the **result here** may be **shown**
  - There are no computations involved
- Showing that **certain lists** are **not linearly independent** and that **certain lists** do *not span* some **given vector space**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206050951205.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206051747129.png)

- Our intuition suggests that **every subspace** of a **finite dimensional vector space** should *also be finite dimensional*
  - Can **prove this intuition**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206051915085.png)

#### Proof (Subspace of finite dimensional vector spaces is finite dimensional)

- Suppose $V$ is *finite dimensional* and $U$ is a **subspace** of $V$ 
  - Must prove that $U$ is *finite dimensional* 
- Do this through the **multi step process** *construction*

##### Step 1 

- If $U = \{0\}$ $\to$ $U$ is *finite dimensional* and we **are done**
- If $U \neq \{0\}$ then choose a **non zero vector** $v_1 \in U$ 

##### Step j

- If $U = span(v_1, \cdots, v_{j-1})$ 
  - Then $U$ is *finite dimensional* and we are **done**
- If $U \neq span(v_1, \cdots, v_{j-1})$ then choose some vector $v_j \in U$ such that:

$$
\large v_j \notin span(v_1, \cdots, v_{j-1})
$$

- After **each step** $\to$ as long as this **process continues**
  - We construct a list of **vectors** such that **no vector** in this list is **in the span of the previous vectors**
- Therefore after **each step** $\to$ we constructed a **linearly independent list** via the *linear dependence lemma*
  - This *list* can **not be longer** than *any spanning list* of $V$ as **proved above**
    - Therefore the **process eventually terminates**
      - This means that $U$ is *finite dimensional*.

> -- self notes --
>
> just recall that finite dimensional means that some list of vectors spans the space which is what the break case is doing
>
> if this is not the case we repeat selecting some vector which is not in the previous span selected but is in $U$ 
>
> ---
>
> Idea is we repeat this and every vector we select being not in the span of its previous ones thus being an linearly independent list
>
> thus relating to the linear independence lemma this list we form can not be longer than any spanning list V
>
> this shows that the process is finite and thus U is finite dimensional.

## Bases

-  Discussed **linearly independent lists** and **spanning lists**
  - Bring this **concept together**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206060205280.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206060222441.png)

- Addition to the **standard basis**
  - $F^n$ has *many other bases*
    - Example:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206060433312.png)

- Next result **explains** *why bases are useful*
  - Recall **uniquely** means **in only one way**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206060505392.png)

### Proof (Basis Criterion)

- Suppose $v_1, \cdots, v_n$ is a **basis** of $V$ 

- Let $v \in V$ 

- As $v_1, \cdots, v_n$ *spans* $V$ 

  - There *exists* $a_1, \cdots, a_n \in F$ such that `2.30` holds

  - To **show that the representation** of `2.30` is **unique**

  - Suppose $c_1, \cdots, c_n$ are **scalars** such that:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206060827612.png)

  - Subtracting from the **last equation** from `2.30` :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206060909559.png)

  - This **implies** that *each* $a_j - c_j$ *equals* $0$ 

    - Because $v_1, \cdots, v_n$ is **linearly independent**.

  - Hence $a_1 = c_1, \cdots, a_n = c_n$ 

    - This is our **desired uniqueness** thus completing the **first direction of proof**

- For the **second direction**

  - Suppose every $v \in V$ can be **uniquely written** in the form given by `2.30` 

    - This *implies* that $v_1, \cdots, v_n$ *spans* $V$ 

  - To show that $v_1, \cdots, v_n$ is **linearly independent**

    - Suppose $a_1, \cdots, a_n \in F$ are **such that**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211206061352383.png)

    - The **uniqueness** of the **representation** `2.30` (taking $v=0$) now **implies** that $a_1 = \cdots = a_n = 0$ 

      - Thus $v_1, \cdots, v_n$ is *linearly independent* and **hence** a *basis* of $V$. QED.

---

- A **spanning list** in a **vector space** *may not be a basis*
  - As its **not linearly independent**.
- The next result $\to$ states that **given any spanning list**
  - Some (*could be none*) of the **vectors** may be **discarded** as to make the **remaining list** is **linearly independent** and *still spans* the **vector space**.

---

- An **example** for vector space $F^2$ 
  - If the **procedure** for the **proof** shown below is applied to:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211208183511073.png)

- Then the **second / fourth** *vectors* are *removed*
  - This leaves the $(1,2), (4,7)$ 
    - This is a **basis** of $F^2$

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211208183655874.png)

---

### Proof (Spanning list contains a basis)

- Suppose $v_1, \cdots, v_n$ *spans* $V$ 
  - We wish to **remove** some of the **vectors** from $v_1, \cdots, v_n$ so the **remaining vectors** form a **basis** of $V$.
- This is done via a **multistep process**
  - Start with $B$ equivalent to the *list* $v_1, \cdots, v_n$ 

#### Step 1

- If $v_1 = 0$  $\to$ *delete* $v_1$ from $B$ 
  - If $v_1 \neq 0 $ then **leave** $B$ *unchanged*.

#### Step j 

- If $v_j$ is in $span(v_1, \cdots, v_{j-1})$ 
  - *Delete* $v_j$ from $B$ 
- If $v_j$ is *not* in $span(v_1, \cdots, v_{j-1})$ 
  - Then **leave** $B$ *unchanged*.

---

- We **stop process** after *step* $n$ $\to$ obtain a list $B$ 
  - This list $B$ *spans* $V$ as the **original list** *spanned* $V$ and we **only discarded** vectors that **were already in the span** of the **previous vectors**
- The *process ensures* that **no vector** in $B$ is in the **span of the previous ones** 
  - Therefore $B$ is *linearly independent* via the **linear independence lemma**
    - Hence $B$ is a **basis** of $V$. QED

---

- The next result $\to$ a **basic corollary** of the **previous result**
  - Tells us that **every** *finite dimensional vector space* has a **basis**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211208185232386.png)

### Proof (Every finite dimensional vector space has a basis)

- By def $\to$ a **finite dimensional vector space** has some **spanning list**
  - The *previous results* tells us that **each spanning list** can be **reduced** to a **basis** QED

---

- The next result $\to$ is a dual of `2.31`
  - States that **every spanning list** can be reduced to a **basis** 
- We show that **given** some *linearly independent list*
  - We can *adjoin* some **additional vectors** $\to$ (*includes* the **possibility** of *adjoining* no **additional vectors**)
    - Therefore the **extended list** is *still linearly independent* but **also** *spans* the *space*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211208185747054.png)

### Proof (Linearly independent list extends to a basis)

- Suppose $v_1, \cdots, v_n$ is *linearly independent* in a **finite dimensional vector space** $V$ 
  - Let $w_1, \cdots, w_n$ be a **basis** of $V$ 
    - Therefore the list:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211208190255063.png)

- Spans $V$
  - Application of *procedure* `2.31` to **reduce** this **list** to *a basis* of $V$ produces a **basis** consisting of the vectors $u_1, \cdots, u_m$ 
    - No $u's$ get **deleted** in this *procedure* because $u_1, \cdots, u_m$ is *linearly independent*
      - Alongside some of the $w's$.  QED

---

- As **an example** for $F^3$ 
  - Say we started with the **linearly independent** list $(2,3,4), (9,6,8)$ 
    - Taking $w_1, w_2, w_3$ in the **proof above** to be the **standard basis** of $F^3$ 
      - The *procedure above* produces the list $(2,3,4), (9,6,8), (0,1,0)$ which is a **basis** of $F^3$ 

---

- Via application of the **above result**
  - We show that **every subspace** of a **finite dimensional vector space** can be *paired* with **another subspace** to form a *direct sum* of the **entire space**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211208191225822.png)

---

### Proof (Every subspace of $V$ is part of a direct sum = $V$ ) 

- Because $V$ is **finite dimensional** , and therefore so is $U$ `2.26` 
  - Therefore a **basis exists** $u_1, \cdots, u_m$ of $U$ `2.32` 
- Clearly $u_1, \cdots, u_m$ is a *linearly independent* list of **vectors** in $V$ (definition of basis.)
  - Thus the **list** may be *extended* to a **basis** $u_1, \cdots, u_m, w_1, \cdots, w_n$ of $V$ `2.33` 
- Let $W = span(w_1, \cdots, w_n)$ 
  - To *prove* that $V = U \oplus W$ via `1.45` , need only **show** that:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211208192006670.png)

- To prove the **first equation**
  - Suppose $v \in V$ 
    - As the list $u_1, \cdots, u_m, w_1, \cdots, w_n$ *spans* $V$ 
      - There *exists* $a_1, \cdots, a_m, b_1, \cdots, b_n \in F$ such that:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211208192213075.png)

- Alternatively speaking, we have $v = u+w$ , where $u \in U$ and $w \in W$ are defined as above.
  - Thus $v\in U + W$.
- To show $U \cap W = \{0\}$ 
  - Suppose $v \in U \cap W$ 
    - There exist **scalars** $a_1, \cdots, a_m , b_1, \cdots, b_n \in F$ such that:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211208192417696.png)

- Thus:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211208192429260.png)

- As $u_1, \cdots, u_m, w_1, \cdots, w_n$ is *linearly independent* 
  - This **implies** that $a_1 = \cdots = a_m = b_1 = \cdots = b_n = 0$ 
    - Therefore $v=0$. QED.

---

## Dimensions

- We have been **discussing** *discussing finite dimensional vector spaces*
  - We have **not defined** the **dimension** of *such an object*
- How *should dimension be defined?* 
  - An **ideal** definition should **force** said dimension of $F^n$ to be **equal** to $n$ 
    - Notice the **standard basis**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211208211745709.png)

- Of $F^n$ has *length* $n$ 
  - Thus we **are tempted** to *define the dimensions* as the **length of a basis**
- However *finite dimensional vector* space in **general has many** *different bases*
  - And our *partial definition* makes sense **only if all bases** in *given vector space* has the **same length**.
    - **Fortunately** that *turns out* to be **the cases** as we **show**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211208212100195.png)

### Proof (Basis length does not depend on basis)

- Suppose $V$ is **finite dimensional** 
  - Let $B_1$ and $B_2$ be the **two bases** of $V$ 
- Then $B_1$ is *linearly independent* in $V$ and $B_2$ *spans* $V$ 
  - Therefore the **length** of $B_1$ is *at most* length of $B_2$ (`2.23`) 
- Interchanging the **roles** of $B_1$ and $B_2$ 
  - Also see that the **length** of $B_2$ is at **most** the **length** of $B_1$ 
    - Therefore the **length** of $B_1$ *equals* the *length* of $B_2$ as **desired.** QED

---

- Now we **know** that **any two bases** of *a finite dimensional vector space* have the **same length**
  - We can **formally define** the *dimensions of such spaces*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209190821803.png)

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209190834784.png)

---

- Every **subspace** of a *finite dimensional vector space* is *finite dimensional* and therefore has a **dimension**
  - The **next result** gives the *expected* about **subspace dimensions**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209192032670.png)

### Proof ($\dim subspace \leq dim \text{ finite dimensional vector space}$)

- Suppose $V$ is *finite dimensional* and $U$ is a **subspace** of $V$ 
  - Think of a **basis** of $U$ as *linearly independent* list in $V$ 
  - Thin of a **basis** of $V$ as a **spanning list** in $V$. 
- Use `2.23` to conclude that $\dim U \leq \dim V$. 

---

- To **check** that a *list of vectors* in $V$ is a **basis** of $V$ 
  - Must according to **said definition** $\to$ show that **the list** in *question satisfies two properties*
    1. Must be **linearly independent** 
    2. Must **span** $V$. 
- The **next two results** show that if **said list** in *question* has the **right length**
  - Then we need **only check** that *it satisfies* one of the **two required properties**
    - First we **prove** that *every linearly independent* list with the **right length is a basis**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209193637658.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209193710803.png)

### Proof (Linearly independent list of the right length is a basis)

- Suppose $\dim V = n$ and $v_1, \cdots, v_n$ is *linearly independent* in $V$ 
  - The list $v_1, \cdots, v_n$ can be **extended** to a **basis** of $V$ `2.33` 
- Every **basis** of $V$ has length $n$ 
  - Thus in this case $\to$ the **extension** is the **trivial one**
    - Meaning no *elements* are **adjoined** to $v_1, \cdots, v_n$ 
  - In other words $v_1, \cdots, v_n$ is a **basis** of $V$. QED.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209194336784.png)

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209194403473.png)

---

- Now we **prove** that a **spanning list** with the **right length** is a **basis**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209195210873.png)

### Proof (Spanning list of the right length is a basis)

- Suppose $\dim V = n$ and $v_1 , \cdots, v_n$ spans $V$ 
  - The list $v_1, \cdots , v_n$ can **be reduced** to a **basis** of $V$ `2.31` 
- However, every **basis** of $V$ has *length* $n$ therefore in this **trivial reduction** that has *no elements* are **deleted** from $v_1, \cdots, v_n$ 
  - In other words $\to$ $v_1, \cdots, v_n$ is a **basis** of $V$ as *desired*.

---

- The **next result** gives a *formulae* for the **dimension** of the **sum of two subspaces** of a *finite dimensional vector space*
  - This **formulae** is *analogous* to a **familiar counting formulae**
    - The *number of elements* in the **union** of **two finite set** *equals* the **number of elements** in the *first set* plus *second set* but **minus** the **number of elements** in the **intersection** of the **two sets**.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209200627769.png)

### Proof (Dimension of sum of subspaces)

- Let $u_1, \cdots, u_m$ be a **basis** of $U_1 \cap U_2$ 
  - Thus $\dim(U_1 \cap U_2) = m$ 
- Because $u_1 ,\cdots, u_m$ is a **basis** of $U_1\cap U_2$ 
  - It is **linearly independent** in $U_1$
    -  Thus the *list* can be **extended** to a *basis* $u_1, \cdots, u_m, v_1, \cdots,v_j$ of $U_1$ `2.33` 
      - Therefore $\dim U_1 = m+j$ 
- Also **extend** $u_1, \cdots, u_m$ to a **basis** $u_1, \cdots, u_m, w_k, \cdots, w_k$ of $U_2$ 
  - Thus $\dim U_2 = m+k$ 

---

- We will show that:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209201034748.png)

- Is a **basis** of $U_1 + U_2$, completing the proof

  - As we will obtain:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209201223906.png)

- The $span(u1_ , \cdots, u_m, v_1, \cdots, v_j, w_j, \cdots, w_k)$ contains $U_1$ and $U_2$ 

  - Thus equals $U_1 + U_2$ 
    - Therefore to **show** that *this list* is a **basis** of $U_1 + U_2$. 
      - Need to show its **linearly independent**, suppose:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209201529648.png)

- Where all $a's, b's$ and $c's$ are **scalars**
  - We need **to prove** that all the $a's, b's$ and $c's$ equal $0$ 
    - Equation above may be **rewritten**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209201630829.png)

- Shows that $c_1w_1 + \cdots + c_kw_k \in U_1$ 

  > self notes
  >
  > because its a linear combination of what the basis of $U_1$ is defined at the start of the proof

  - All the $w's$ are in $U_2$ 
    - Therefore this **implies** $c_1w_1 + \cdots + c_kw_k \in U_1 \cap U_2$  
      - Because $u_1, \cdots, u_m$ is a **basis** of $U_1 \cap U_2$ , we write:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209201901271.png)

- For *some choice of scalars* $d_1 , \cdots, d_m$ 
  - But $u_1, \cdots, u_m, w_1, \cdots w_k$ is *linearly independent*
    - Thus for the **last equation** it *implies* that all the $c's$ (and $d's$) equal $0$.
      - Thus our **original equation** *involving* the $a's, b's$ and $c's$ becomes:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211209202030471.png)

- Because the *list* $u_1, \cdots, u_m, v_1, \cdots , v_j$ is *linearly independent*
  - This equation implies that all $a's$ and $b's$ are $0$
    - We now **know** that all $a's, b's$ and $c's$ equal to $0$ as **desired**. QED.

