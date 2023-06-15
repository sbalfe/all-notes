# Linear algebra

## Determinants

- Rather than think of a **solution** of **linear equations** 
  - For our **purpose** $\to$ think of the **determinants** as **another way** to *multiply vectors*.

---

- In **2D** vectors $a$ and $b$ the *determinant* $| ab |$ is the **area** of the **parellogram** found by $a$ and $b$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106141516397.png)

- This is **a signed area** $\to$ the sign **is positive** if  $a$ and $b$ are **right handed** and **negative** if **left handed**.
  - This means $| ab | = - | ba |$ 
- In **2D** we *can interpret* as **right handed**
  - As *meaning* we **rotate** the **first vector** *counterclockwise* to *close* the **smallest angle** to the **second vector**.
- In **3D** the determinant is taken with **all three vectors** at a *time*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106215735905.png)

- For **3D vectors** $a,b,c,$ 
  - The **determinant** $| abc|$ is the **signed volume** of the of the *parallelepiped* (**3D parallelogram** / **sheared 3D box**) formed by **three vectors** shown above.

---

- To compute a **2D determinant** 
  -  First *establish* a *few* of **its properties**
    - Note that **scaling** *one* side of a **parallelogram** *scales* its **area** by the **same fraction**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106221045660.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106220741807.png)

- Also note that *shearing* a **parallelogram** does **not change** its area

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106222141248.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106220853601.png)

- Finally see the **determinant** has the **following property**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106221010412.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106222756780.png)

- Because as **shown** in figure below, the **edge** between the **two parallelograms** over to **form** some **single parallelogram** without *changing* the **area** of *either* of **two original** *parellograms* 
  - Assume a **cartesian representation** for $a$ and $b$: 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106222102928.png)

- Simplification $\to$ use the **fact** that $|vv| = 0$ for **any vector** $v$ 
  - As the **parallelograms** would all be *collinear* with $v$ and thus **without** *area*
- In **three dimensions** $\to$ the *determinant* of **three** *3D* vectors of $a,b,c$ denoted as $|abc|$ 
  - For the **cartesian representation** of the **vectors** 
    - There are **analogous rules** for **parallelepipeds** as there are **for parallelograms**
      - Then do *analogous* expansion as done for **2D**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106225307007.png)

- As you **can see** the **computation** of *determinants* gets **uglier** as **dimensionality increases**
  - Discuss the **less error prone** methods of **computation later**

#### Example

- Determinants arise naturally when **computing** the **expression** for one *vector* as a **linear combination** of the *two others*
  - If we wish to express a vector $c$ as a **combination** of vectors $a$ and $b$:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106225836312.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106225956093.png)

- From **figure 5.6**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106230126197.png)

- Because these **parallelograms** are just **sheared versions** of *each other* (idea is that the area doesnt actually change when one factor is applied to either of the original vectors)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211107221603603.png)

  - Solving for $b_c$ yields:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106230156984.png)

- *Analogous arguments* yields:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211106230257073.png)

- This is the **2D Cramers rule**.

https://www.youtube.com/watch?v=jBsC34PxzoM

## Matrices

- Example matrix
  - Array of **numeric elements** that follow *certain arithmetic rules*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211107213137103.png)

- Use in CG for representing **spatial transforms**

### Vector operations in matrix form

- For graphics we use a **square matrix** to *transform* a vector **represented** as a **matrix**
  - Example $\to$ If you have some **2D vector** $a = (x_a, y_a)$ 
    - Wish to **rotate** it by **90 degrees** about the **origin** to form vector $a' = (-y_a,x_a)$ 
- Use a **product** of a $2 \times 2$ matrix and $2 \times 1$ matrix
  - Called a **column vector**
    - The **operation** in matrix form:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211107223519524.png)

- Obtain the **same result** via use of the **transpose** of **this matrix**
  - Then **multiplying** on the **left** (*premultiplying*) with some **row vector**.:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211107223635242.png)

- *Post multiplication* is the **standard however**
  -  Only difference is the **transposition** of the **rotation matrix**.

---

- Use the **matrix formalism** to *encode* operations on **just vectors**

  - Consider the **result** of the **dot product** as $1 \times 1$ matrix
    - Can be **written**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211107223947934.png)

- Taking **two 3D vectors**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211107224007947.png)

---

- The **outer product** is also defined as a **vector product**
  - Expressed as a **matrix multiplication** with some **column vector** on the **left** and **row vector** on the **right**: $ab^{T}$ 
    - Result is a **matrix** consisting of **all pairs** of an **entry** of $a$ with an **entry** of $b$ 
      - For **3D vectors**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211107224246885.png)

- Think of **matrix multiplication** in terms of **vector operations**
  - Illustrate our **3D** case
    - Think of a $3 \times 3$ matrix as a **collection** of **three 3D vectors**:
      1. made of **three column vectors** that are **side by side**
      2. made of **three row vectors** *stacked up* 
- The **result** of a **matrix vector** *multiplication* $y=Ax$ can be **interpreted** as a *vector* whose entries are **the dot products** of $x$ with the **rows** of $A$ 
  - Naming these **rows** *vectors* $r_i$ , we obtain:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211107225152436.png)

- *Alternatively* $\to$ think of the **same product** as a **sum** of the **three columns** $c_i$ of $A$
  - *weighted* by the **entries** of $x$: 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211107225409988.png)

- Using **same ideas** $\to$ understand a **matrix - matrix product** $AB$ 
  - As some *array* containing the *pairwise dot products* of **all rows** of $A$ with **all columns** of $B$
  - As a *collection* of *products* of the **matrix** $A$ with all the **column vectors** of $B$, arranged **left / right**.
  - As a *collection* of *products* of **all row vectors** of matrix $A$ with the **matrix** $B$ , *stacked* **top to bottom**.
  - As the **sum** of the *pairwise outer products* of **all columns** of $A$ with all rows of $B$. 

- The **interpretations** of *matrix multiplication* often lead to **valuable geometric interpretations** 

---

###  Special types of matrices

- The **identity matrix** is an **example** of a *diagonal matrix* where all **non zero elements** occur **along the diagonal** 
  - The *diagonal consists* of *those elements* whose **column index** is *equivalent* to **the row index** counting from the **upper left**. 

- Has the **property** that its the **same** as the **transpose** 
  - These are **symmetric** 
- Identity is **also** an *orthogonal* **matrix** 
  - Because **each** of its **columns** *considered* as a **vector** has **length** $1$ and the **columns** are *orthogonal* to **one another** 
- Same holds for the **rows**
  - The **determinant** of *any orthogonal* matrix is either $+1$ or $-1$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211107233229042.png)

- Very of **useful property** of *orthogonal matrices* is that they **nearly** on *their own* inverses
  - **Multiplying** an **orthogonal matrix** by its **transpose** *results* in **the identity**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211107233358109.png)

- Easy to **see** because the **entries** of $R^TR$ $\to$ **dot products** *between* the **columns** of $R$ 
  - *off - diagonals* entries are **dot products** between **orthogonal vectors**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211107233719615.png)

## Computing with matrices and determinants

https://www.youtube.com/watch?v=Ip3X9LOh2dk

https://www.youtube.com/watch?v=umbJOv42pE8 - why determinant of A

- Determinant takes $n \times n$ vectors and combines them to obtain **signed** $n$ *dimensional* volume of $n$ *dimensional* **parallelepiped**  
  - Example $\to$ the **determinant** in **2D** is the **area** of the **parellogram** formed by *the vectors*
    - We can use **matrices** to **handle** the *mechanics* of **computing determinants**
- If we have **2D vectors** $r$ and $s$ 
  - Denote the **determinant** $|rs|$ 
    - This is the **signed area** of the **parallelogram** formed by the **vectors**.
- Say we have **2D vectors** with cartesian coordinates $(a,b)$ and $(A,B)$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108205312454.png)

- Determinant written **with regards** to the **column vectors** as a **shorthand**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108205402349.png)

- The **determinant** of a matrix is the **same** as the **determinant** of its **transpose**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108211833972.png)

- Means for **any parallelogram** in *2D* 
  - There is some *sibling* **parallelogram** with the **same area** but a **different shape**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108211934227.png)

- Example $\to$ the *parallelogram* defined via **vectors** $(3,1)$ and $(2,4)$ has area $10$ 
  - So does the **parallelogram** defined by vectors $(3,2)$ and $(1,4)$ 



---

##### Example

- The **geometric meaning** of the **3D determinant** helpful in *seeing* why some formulaes **make sense**
  - Example $\to$ the **equation** of the **plane** through points $(x_i, y_i, z_i)$ for $i = 0,1,2$ :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108212309117.png)

- Each **column** is a **vector** from *point* $(x_i, y_i, z_i)$ to *point* $(x,y,z)$.
  - The **volume** of the **parallelepiped** with *those vectors* as **sides** is **zero** *only if* $(x,y,z)$ is **coplanar** (within the same plane) 

- Almost **all equations** that involve *determinants* have **similarly** *underlying geometry.*

---



- Can compute **determinants** with *brute force expansion*
  - Where **most terms** are *zero* and confusing **plus / minus signs**
- Standard method of **managing** the **algebra** of *determinant computations* is **==Laplace's expansion==** 
  - Key method of **determinant computation** is to find the *cofactors* of **various matrix elements**

- Every element of a **square matrix** has some *cofactor* which is the **determinant** of a **matrix** with *one fewer row* and *column* possibly **multiplied by minus one**
  - The *smaller matrix* is obtained by **eliminating the row** and **column** that the *element* in **question** is in
    - Example $\to$ $10 \times 10$ matrix
      - The **cofactor** of $a_{82}$ is the **determinant** of the $9 \times 9$ matrix with the **8th row** and **2nd column eliminated** 
        - The *sign of the cofactor* is **positive** if the *sum* of the **row and column** indices is *even*, *negative* **otherwise**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108213437900.png)

- For some $4 \times 4$ matrix

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108213455964.png)

- *Cofactors* of the **first row**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108213514712.png)

- The *determinant* of a **matrix** found via taking the **sum** of the **products** of the **elements** of *any row / column* with its **cofactors**.
  - Example the **determinant** of the $4 \times 4$ matrix above taken **its second column**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108213650227.png)

- This is a **recursive expansion**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108213953564.png)



- Can *deduce* the **volume** of the *parallelepiped* formed by **the vectors** defined by the **columns** (*rows* as the **determinant** of **transpose** is the **same**) is **zero**.

- Equivalent statement saying that **columns / rows** are *not linearly independent*
  - Note the **sum** of the **first / third rows** is *twice* the **second row** $\implies$ *linear independence*.

## Linear systems

- Encounter **linear systems** often in *graphics* with $n$ *equations* and $n$ *unknowns*
  - Often for $n = 2$ and $n = 3$

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108214628646.png)

- $x,y,z$ are the **unknowns** of what we **wish to solve**
  - Write in *matrix form*:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108214744852.png)

- Shorthand is $Ax=b$ where its assumed:
  - $A$ is a **square matrix** with *known constants*
  - $x$ is an **unknown column vector** (*our example* has $x,y,z$ of course.)
  - $b$ is a **column matrix** of *known constants*
- Various *methods* of **computation**
  - Depends on the **properties** / **dimensions** of the *matrix* $A$ 
- In graphics we often work with **systems** of size $n \leq 4$ 
  - Discuss a **method** appropriate for **systems** known as the *cramers rule*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108215106399.png)

- The *rule* being take a **ratio of determinants** 
  - Where the *denominator* is $|A|$ and *numerator* as the **determinant** of *matrix* via replacing a **column** of $A$ with the **column vector** $b$ 
    - The **column replaced** will *correspond* to the **position** of the **unknown** in *vector* $x$ 
      - Example $\to$ $y$ is the **second unknown** thus **second column** is *replaced*.

- Note if $|A|$ = 0 there is **no solution** as its **undefined**
  - Another version of the rule that if $A$ is **singular** (*zero determinant*)
    - There is **no unique solutions** to the **equations**

https://www.youtube.com/watch?v=jBsC34PxzoM - geometric visualisation of the cramers rule.

## Eigenvalues and Matrix diagonalization

https://www.youtube.com/watch?v=PFDu9oVAE-g&

- Square **matrices** have *eigenvalues* or *eigenvectors* associated with them
  - The *eigenvector* are the **non zero** *vectors* whose **directions** *do not change* when **multiplied** by some **matrix**
    - Example $\to$ suppose for *some matrix* $A$ and vector $a$:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108221359001.png)

- We have **stretched / compressed** $a$ but the **direction** has *not changed*
  - **scale factor** $\lambda$ is the *eigenvalue* associated with *eigenvector* $a$.

---

- Knowing the **eigenvectors** of *matrices* is useful in many **practical applications**
  - Describe as to **gain insight** into **geometric transformations** as a *step toward* *singular values* and *vectors* described in the **next section**

- Assuming a **matrix** has at **least one eigenvector**
  - We can do a **standard manipulation first**.
    - Write *both sides* as the **product** of a **square matrix** with vector $a$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108221815718.png)

- Where $I$ is the **identity matrix** , rewritten:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108222720826.png)

- As **matrix multiplication** is *distributive* , *group* the *matrices*:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108222808413.png)

- Equation *only holds* if the matrix $(A - \lambda I)$ is *singular*
  - Therefore its **determinant** is *zero*.
    - The **elements** in this **matrix** are *numbers* in $A$ except **along the diagonal**
      - Example $\to$ a $2 \times 2$ matrix $\to$ *eigenvalues* *obey*:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108222941388.png)

- There are **two solutions** of $\lambda$ therefore
  - May / May not be *real / unique* 

---

- Similar **manipulation** for some $n \times n$ matrix $\to$ yields an $n^{th}$ **degree polynomial** in $\lambda$ 
  - Its not **possible** , *in general* to find exact **explicit solutions** of **polynomial equations** of *degree* $ > 4$ 
    - Can *only compute eigenvalues* of matrices $4 \times 4$ or **smaller** via **analytic methods**. 

https://www.youtube.com/watch?v=9aUsTlBjspE - proof of this idea, group theory and galois field shit.

- For **larger matrices** $\to$ *numerical methods* are the **only option**
  - Important **special case** $\to$ where *eigenvalues* and *eigenvectors* are **simple** is **symmetric matrices** ($A$ = $A^{T})$ 
- The *eigenvalues* of **real symmetric matrices** are *always* **real** numbers
  - If they are also **distinct** , the *eigenvectors* are **mutually orthogonal**
    - These matrices can be **placed** into **==diagonal form==** 
    - https://en.wikipedia.org/wiki/Orthogonal_matrix - orthogonal matrices

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108230500318.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108230703995.png)

- Where $Q$ is an **orthogonal matrix** and $D$ is a **diagonal matrix**
- The **columns** of $Q$ are the *eigenvectors* of $A$ 
- The **diagonal elements** of $D$ are the *eigenvalues* of $A$
- Placing $A$ in this form is **==eigenvalue decomposition==** 
  - It *decomposes* $A$ into a **product** of *simpler matrices* to reveal its **eigenvectors** and **eigenvalues**.

https://math.stackexchange.com/questions/82467/eigenvectors-of-real-symmetric-matrices-are-orthogonal

https://www.youtube.com/watch?v=HWnCv4iHCDc

https://www.youtube.com/watch?v=WTLl03D4TNA - diagonalization 

https://www.youtube.com/watch?v=PKDI8Zl1rlc - not diagonalization.

https://www.youtube.com/watch?v=Lq7a8hNUzYI - more diagonalization 

https://en.wikipedia.org/wiki/Spectral_theorem 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109040131475.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109040256394.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109161418411.png)

##### Example

- Given some *matrix*:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108231306552.png)

- The *eigenvalues* of $A$ are the **solutions** to

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108231328996.png)

- *Approximate* the **exact values** for *compactness* of **notation**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108231418273.png)

- Find the **associated** *eigenvector* 
  - First one is *non trivial* solution to the **homogenous equation**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108231522727.png)

- Approximately $(x,y) = (0.8507, 0.5257)$ 
  - There are **infinitely many solutions** that are *parallel* to that **2D vector**
    - Just selected one of **unit length**
- The *eigenvector* associated with $\lambda_2$ is $(x,y) = (-0.5257, 0.8507)$ 
  -  Therefore the **diagonal form** of $A$ (*WITHIN PRECISION* via our approximation):

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211108231758945.png)

## Singular value decomposition

- Any *symmetric matrix* can be **diagonalized** / *decomposed* to **convenient** product of **orthogonal / diagonal matrices**
- Most *matrices* in **graphics** are *not symmetric* 
  - The **eigenvalue decomposition** for *nonsymmetric matrices* is **not convenient** or **illuminating**
    - Involves **complex valued** *eigenvalues* and *eigenvectors* even for **real valued inputs**.

---

- Another *generalisation* of the **symmetric eigenvalue decomposition** to **nonsymmetric** (*even non square*)
  - Its the **singular value decomposition**
    - The difference between the **eigenvalue decomposition** of a **symmetric matrix** and the **SVD** of **nonsymmetric matrix** is that the **orthogonal matrices** on the **left / right** are *not required* to be the *same* in **SVD**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109030002628.png)

- $U$ and $V$ are **two** *potentially different* **orthogonal matrices**
  - Whose columns are **known** as the **left / right** *singular vectors* of $A$ 
- $S$ is the **diagonal matrix** whose entries are **singular values** of $A$
  - When $A$ is **symmetric** with *nonnegative eigenvalues*
    - The **SVD** and the **eigenvalue decomposition** are the *same*.

---

- Another *relationship* of **singular values** and **eigenvalues** that can be used to **compute** the **SVD** 
  - Define $M = AA^{T}$ 
    - Assume that we can perform some **SVD** on $M$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109030715427.png)



- The **substitution** is based on the *fact* that $(BC)^T = C^TB^T$ 
  -  The **transpose** of an **orthogonal matrix** is *its inverse* 
  - The **transpose** of a **diagonal matrix** is the **matrix itself**

---

- This form shows that $M$ is **symmetric** and $US^2U^T$ is its **eigenvalue decomposition**. 
  - Where $S^2$ contains the **eigenvalues** (*non negative*) 
    - Therefore we find that **the singular values** of a **matrix** are the **square roots** of the **eigenvectors** of the **product** with its **transpose** 
    - The **left singular vectors** are the **eigenvectors** of that *product*
- Similar argument allows $V$ 
  - The matrix of **right singular vectors** to be **computed** from $A^TA$  

- So we have basically used the idea that $AA^T$ is still diagonalizable and therefore we obtain a form similar to eigenvalue decomp but the eigenvalues are the squared middle therefore we obtain the singular values via taking the square root.

---

![img](https://cdn.discordapp.com/attachments/540211747613704221/907631257461080164/467435887236612106.png)

https://www.youtube.com/watch?v=TX_vooSnhm8&list=PL221E2BBF13BECF6C&index=63 - singular value decomposition

https://www.youtube.com/watch?v=JfqZdTVsvso - why orthogonal matrices transposes are its identity matrix.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109162335180.png)

- $\sigma_i$  = $i_{th}$ singular value
- For **symettric matrices** $\to$ the **eigenvalues** and the **singular values** are the *same* 