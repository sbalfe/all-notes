# Strong Normalization Coursework

- Vital theorem for $\lambda$ calculus $\to$ **simply typed terms** $\to$ are *strong normalizing*
  - Every reduction path **eventually terminates**.
    - This *not trivial to prove*.
- For this CW $\to$ we consider a formulate **proof method**
  1. Calculate the **upper bound** to the the **length** of *all possible reductions* of a *typed term*
  2. Prove that this **bound** *always reduces* when a **reduction step** is applied
- Here we implement the **first part**, calculation of the **upper bound**
  - No formal proof for the second part but we **observe** it in examples.

## Simply typed terms

- Extend grammar of $\lambda$ calculus of tutorials with *simple types*
- Type obtained by following **grammar**
  - $o$ is the **base type**.
  - $o \to \tau$ is an **arrow type**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220323160351385.png)

- Terms of **simply typed** $\lambda$ calculus are given by the **following grammar**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220323160418082.png)

### Assignment 1

#### a)

- Complete datatype $Type$ to represent **simple types** following the **grammar above**
- For the **base type** $\to$ use the *constructor* $Base$ 
- For **arrow type** $\to$ use the *infix constructor* $:->$ 
- *Uncomment* the `nice` function, `Show` instance and examples `type1` and `type2` to see everything type checking.

# Type checking

## Assignment  2

- Types only have annotation at them moment
- Must be **well typed**
  - The types of *functions* and *arguments* are matched in the **expected way**
    - Provide a **simple inductive algorithm** implemented here.
- This is *not type inference* which is the **more involved algorithm** that decides whether **an untyped** term can be **given a type**

---

- Define as such

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220329131701811.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220329131753003.png)

- Type checking rules give us **inductive algorithms**
  - To find some type $M$ 
    - The inputs of **context** and **M** 
      - Algorithm outputs either a type $\tau$ If $M$ is **well typed** or fails if **M** is *not well typed*
- The *conclusion* of **rule** is *what is computed*, premises of a rule gives the **recursive calls**.

---

## Assignment 3 - Higher order numeric functions

- Construction for counting **reduction steps**
- Build a function from **simply typed terms** to **natural numbers** so when some **term reduces**, the *number* gets *smaller*

> Follows that all reduction paths must eventually end, and that that the term is strongly normalizing

---

- Problem is an **application**
  - Suppose some term $MN$ 
    - We know $M$ reduces in at most $m$ steps and $N$ it at most $n$ steps.
    - This does *not help* us figure out the steps for $MN$. 
    - If $M$ and $N$ are *normal* (at most zero reduction steps), $MN$ may **not be normal**.
- Solution $\to$ give $M$ not a *number* but rather a **function** that provided a *bound* for $N$ , produces a *bound* for $MN$ 

---

- The types ensure that **everything works out** 
  - We build a function that **interprets** terms as *==functionals==* $\to$ *higher order functions over numbers*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331203705408.png)

- This provides us a **higher order function** over *numbers*, but does not yet provide a number.
  - To obtain a *number* we evaluate the *functional* with **==dummy arguments==** 
    - $0$ for $\N$ 
    - $g(x) = 0$ for $\N \to \N$   

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204038004.png)

- We start **making these ideas** more precise

---

- The set of **functionals** we need are provided by the following grammar

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204205649.png)

- The function $|\tau|$ takes every type $\tau$ to a set **of functionals**, defined by:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204238428.png)

- Since a type is of the form:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204331760.png)

- A set of *functionals*  that type is of the following form

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204403224.png)

---

- For every type $\tau$ , the **dummy element** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204528710.png)

- Defined by:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204539661.png)

- Informally if 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204548556.png)

- Then the dummy element

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204556964.png)

- Takes $n$ argument , discards them all and returns *zero*.

---

- For some functional $f \in |\tau|$ , the **counting operation** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204711901.png)

- Returns a number by providing the **necessary dummy arguments** , defined by

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204747572.png)

- Informally, if 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204757886.png)

- Then for 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204806108.png)

- the counting operation gives the following 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204820026.png)

---

- The **increment operation** function 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204848196.png)

- Increments a functional $f \in |\tau|$ by a number $n$ defined by

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204911997.png)

- Informally for

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204920654.png)

- And any functional in the set

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220331204933205.png)

- The increment function $+_\tau$ adds a number to the *last* $\N$.

# Assignment 4

- To give an upper bound to the *number* of *reduction steps*, define some function $||M||$ 
  - This takes a term $M : \tau$ to a function $f \in |\tau|$ 

> This is a straightforward induction on $M$ 

- As with **the type checking function** , where we require a context $\Gamma$ to know the **free variable types** of $M$.
- We need a **==valuation==** which assigns to each variable $x: \tau$ a *functional* $f \in |\tau|$ 

----

- We write $v$ for a valuation and $v [x \mapsto f]$ for the valuation that maps $x$ to $f$ and **any other variable** $y$ to $v(y)$. 

---

- The *interpretation* of $M$ is then *constructed as follows*

  1. If $M$ is a variable $x:\tau$ , look up the functional in $v$, returning $v(x) \in |\tau$ 
  2. For an **abstraction** $\lambda x^{\sigma}.M: \sigma \to \tau$. Construct some **functional** $f \in |\sigma| \to |\tau|$ as follows.
  
     - For any $g \in |\sigma|$ , define $f(g)$ to be **to be interpretation** of $M : \tau$ for the valuation $v[x \mapsto g]$ 
     - Consider $x$ in $M$ to represent an **arbitrary term**, then $g$ represents the bound on the **reduction steps** for $x$. 
     - $f$ uses $g$ to compute the bound  for $M$ 
  3. For an **application**
