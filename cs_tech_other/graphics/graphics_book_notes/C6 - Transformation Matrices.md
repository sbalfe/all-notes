# Transformations matrices

- *Geometric transformations* such as **rotation / translation / scaling / projection**

  -  Accomplished with **matrix multiplication**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109173855352.png)

## 2D Linear Transformations

- Use a $2 \times 2$ matrix to **transform** some **2D vector**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109174009415.png)

- Takes in some **2-vector** to produce **another** **2-vector** via *simple matrix multiplication*
  - This is a **==linear transformation==** 
- Consider moving on $x$ as a **horizontal move**, $y$ **vertical move**.

### Scaling

- Most basic **transform** $\to$ to *scale* along **some coordinate axes**
  -  Transform can **change length** and **possibly direction**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109174235086.png)

- What this does to a **vector** with **cartesian components** $(x,y)$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109175525583.png)

- Just by **looking** at the **matrix** of an **axis-aligned scale** $\to$ read off the **two scale factors**.

https://stackoverflow.com/questions/7303787/what-does-axis-aligned-mean

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109175653699.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109175704867.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109175835154.png)

### Shearing

- A **shear** just *pushes* things **sideways**
  - Produces something similar to that of a **deck of cards** across which **you push your hand**
    - Bottom card stays put and cards **move more** the *closer* they are the **to the top**
- *Horizontal / Vertical shear matrices* are:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109180320799.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109180354374.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109180545700.png)

- An *analogous* transform **vertically** is shown above:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109180704606.png)

- For **both cases** 
  - The *square outline* of the **sheared clock** becomes a *parallelogram* 
    - The **circular face** of the **sheared clock** $\to$ becomes an **ellipse**.
- Alternative understanding of a **shear** is in terms of **rotation** of only the **vertical (horizontal)** *axis*
  - The *shear transform* that takes a **vertical axis** and **tilts** *clockwise* by angle $\phi$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109181150208.png)

- *Similarly* the **shear matrix** rotating the **horizontal axis** *counterclockwise* by angle $\phi$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109181407477.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110135238206.png)

### Rotation

- Want to **rotate** a **vector** $a$ by an angle $\phi$ **counterclockwise** $\to$ obtain vector $b$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109181857703.png)

- If $a$ makes an **angle** $\alpha$ with the $x$ - **axis** with *length* $r = \sqrt{x^2_a+y^2_a}$ 
  - Then we **know** that:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109182009645.png)

- Because $b$ is a *rotation* of $a$ 
  - Has **length** $r$ 
- Because it is **rotated** an angle $\phi$ from $a$ 
  -  $b$ makes angle $(\alpha + \phi)$ with the $x$ axis
    - Using **trigonometric** *addition identities*:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109183035024.png)

- *Substituting* $x_a=r \cos \alpha$ and $y_a = r \sin \alpha$ gives:



![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109183221998.png)

- In the **matrix form** $\to$ the **transformation** that takes $a$ to $b$:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109185437253.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109185451630.png)

- A **matrix** that rotates $\frac{\pi}{6}$ **radians** (*30 degrees*) in the *clockwise direction* 
  -  Is a **rotation** by $- \frac{\pi}{6}$ **radians** in *our framework*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109185823530.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109185941127.png)

- The **norm** of *each row* of a **rotation matrix** is **one**: $(\sin^2 \phi +cos^2\phi =1)$ 
  - Alongside the **rows** being **orthogonal** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109190334633.png)

- We view that **rotation matrices** are **orthogonal matrices**
  - By *Looking* at the **matrix** $\to$ can **read** off **two pairs** of *orthonormal vectors* 
    - The **two columns**
      - *Vectors* to which the **transformation** sends the **canonical basis vectors** $(1,0)$ and $(0,1)$ 
    - And the **rows** which are the **vectors** that sends **to** the **canonical basis vectors**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109191647017.png)

### Reflection

- *Reflect* some **vector** across **either** of the **coordinate axes** by using a *scale* with **one negative scale factor**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109191553408.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109191605355.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109191751043.png)

- One **might expect** that the **matrix** with $-1$ in **both elements** of the **diagonal** is *also a reflection*
  - In fact its a **rotation** by $\pi$ *radians*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109192059315.png)

### Composition / Decomposition of Transformations

- Common for **graphics programs** to *apply* more than **one transformation** to an **object**
  - Example $\to$ may first apply a **scale** $S$ $\to$ then **rotation** $R$ 
    - Performed in **two steps** on a **2D** vector $v_1$:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109192340740.png)

- Alternative way to write this:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109192428199.png)

- As **matrix multiplication** is *associative* $\to$ write:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109192521930.png)

- In **other words** $\to$ represent the **effects** of **transforming** a *vector* by **two matrices** in *sequence* using a **single matrix** of the *same size*
  - We can **compute** by **multiplying** the *two matrices* $M=RS$ 
- Important to **remember** that *these transforms* are **applied** from the **RHS** first
  - $S$ before $R$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109194530770.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109194606402.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109194616826.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109194729710.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109194812718.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109194823121.png)

- **Building** up a **transformation** from *rotation / scaling* **transformations** *actually works* for **any linear transformations**
  - This **fact** *leads* to a **powerful way** of *thinking* about **these transformations**.

### Decomposition of Transformations

- Sometimes we wish to **undo** a *composition* of **transformations**
  - Taking a **transformation** apart to **simple pieces**
- For *instance* $\to$ useful to **present** *a transformation* to the **user** for **manipulation** in regards to **seperate rotations** and **scale factors**
  - However a **transformation** may be *represented* **internally** *simply* as a **matrix** with *rotations / scales* already **mixed together**.
- This kind of **manipulation** is achieved if the **matrix** can be **computationally** *dissembled* into the **desired pieces**
  - *Adjust said pieces* 
    - Then *reassemble* by **multiplying** the **pieces together again**.
- **Decomposition** / **Factorization** is possible *regardless* of the **entries** in the **matrix**
  - Provides a **useful** way of **thinking about transformations** and what **they do** to the **geometry** that is **transformed by them**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109195617712.png)

#### Symmetric Eigenvalue Decomposition

https://www.youtube.com/watch?v=EokL7E6o1AE - this

- Start with **symmetric matrices**
  - Some **symmetric matrices** can *always* be taken **apart** using the **eigenvalue decomposition** to a **product** of this form:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109200656819.png)

- Where $R$ is an **orthogonal matrix** , $S$ is a **diagonal matrix** 
- Call **columns** of $R$ (*eigenvectors*) by the names $v_1$ and $v_2$ 
- Call **diagonal entries** of $S$ (*eigenvalues*) by names $\lambda_1$ and $\lambda_2$ 

---

- In *geometric terms* $\to$ recognize $R$ as **rotation** and $S$ as **scale**
  - This is simply a **multistep** *geometric transformation*
    1. **Rotate** $v_1$ and $v_2$ to the $x-$ and $y-$ axes (the **transform** by $R^T$) 
    2. **Scale** $x$ and $y$ by $(\lambda_1, \lambda_2)$ (the **transform** by $S$)  
    3. **Rotate** $x-$ and $y-$ axes back to the $v_1$ and $v_2$ (the **transform** by $R$)

---



- Viewing the **effect** of the **three transforms** together
  - See that they have the **effect** of a **nonuniform scale** along some *pair* of *axes*. 
    - As with **axis-aligned scale** $\to$ the *axes* are **perpendicular** 
      - But they are **not the coordinate axes** 
        - They are the **eigenvectors** of $A$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109202358655.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109202555994.png)

- This *tells* us something about **what it means** to be a **symmetric matrix**

  - They are simply **scaling operations**
    - potentially **nonuniform** and **non-axis** *aligned ones*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109202639743.png)

##### Example 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109203440118.png)

- The **matrix above** according to its **eigenvalue decomposition** 
  - *Scales* in the **direction** $31.7^{\circ}$ *counterclockwise* from **3 o clock**
- Can **reverse** the **diagonalization**  process also
  - To *scale* $(\lambda_1, \lambda_2)$ with the **first scaling direction** 
    - An angle $\phi$ *clockwise* from the $x$ axis $\to$ we **have**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109203749249.png)

- Take to **account** this is a **symmetric matrix** as we *know* must be **true** since we **constructed** from a **symmetric eigenvalue decomposition**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109204016862.png)

#### Singular Value Decomposition

> A matrix is said to be rank deficient if it doesn't have full rank.
>
> When a matrix is not full rank, it means that at least one column of your matrix depend on other column in the matrix. In this case one column can be written by combination of two other columns or one column multiple to a constant number generate one of the other columns.

- A **similar kind** of **decomposition** can be *done* with **non-symmetric** *matrices*
  - Its the **==singular value decomposition==** **SVD**
    - The **difference** is that the *matrices* on **either side** of the **diagonal** *matrix* are **no longer the same**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109213005127.png)

- The **two orthogonal matrices** that *replace* this single rotation $R$ are called $U$ and $V$ 
  - Their **columns** are *called* $u_i$ (*left singular vectors*) , $v_i$ (*right singular vectors*)
- In **this context**
  - The **diagonal entries** of $S$ are **called** *singular values* rather than **eigenvalues**

---

- The **geometric interpretation** is *similar* to that of **symmetric** *eigenvalues* *decomposition*
  1. **Rotate** $v_1$ and $v_2$ to the $x-$ and $y-$ axes (the **transform** by $V^T$) 
  2. **Scale** $x$ and $y$ by $(\sigma_1, \sigma_2)$ (the **transform** by $S$)  
  3. **Rotate** $x-$ and $y-$ axes back to the $u_1$ and $u_2$ (the **transform** by $U$)

---

- The **principle difference** is *between* a **single rotation** and **two different orthogonal matrices**. 
  - This creates *some other less important difference* 
    - As the **SVD** has *different singular values* on **two sides** 
      - There is **no need** for **negative singular values**
      - Therefore we can **always flip** the *sign* of the a **singular value** / *reverse* the **direction** of *one* of the **associated** *singular vectors* and **end with** the exact **same transformation again**.
    - For this reason $\to$ **SVD** always produces a **diagonal matrix** with *all positive entries* 
- Matrices $U$ and $V$ are **not certain** to be **rotations**
  - Could involve **reflections**
    - For **geometric applications** $\to$ this is **an inconvenience** although a *minor one* 
      - Its **easy** to *differentiate* the **rotations** from **reflections** via checking the **determinant** which is $+1$ for **rotations**, $-1$ for **reflections**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109214315916.png)

- If **rotations are desired** 
  - Then *one* of the *singular values* may be **negated** 
    - Thus resulting in a **rotation - scale - rotation** *sequence* where the **reflection** is *rolled* in with the **scale**
      - Rather than **one of the rotations**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109214458369.png)

- The **immediate** *consequence* of the **existence** **SVD** 
  - Is that **all** the **2D transformation matrices** we have seen can be **made** from **rotation matrices** and **scale matrices** 

- Shear matrices are a **convenience** 
  - They are **not required** for **expression transformations**.

---

###### Summary 

- Every **matrix** may be *decomposed* $\to$ via **SVD** into a **rotation times** a *scale* **times** *another rotation*
- Only **symmetric matrices** may be **decomposed** via **eigenvalue diagonalization** to a **rotation** *times* a **scale times** the *inverse - rotation* 
  -  And **such matrices** are a **simple scale** in some **arbitrary direction** 

- The **SVD** of a **symmetric matrix** will *yield* the **same triple product** as *eigenvalue decomposition* 
  - Via a **slightly** *more complex* **algebraic manipulation**.

#### Paeth Decomposition of Rotations

- Another **decomposition** uses *shears* to *represent* **non zero** *rotations*
  - Following **identity** enables this:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109223703573.png)

- Example $\to$ rotation of $\frac{\pi}{4}$ degrees is:  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211109224046403.png)

- This **particular** *transform* is **useful** for **raster rotation** 
  - Because **shearing** is *a very efficient raster operation* for **images**
    - Introduces some **jagginess**.
      - But **leaves no holes**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110082553684.png)

- Key **observation** $\to$ taking some **raster position** $(i,j)$ 

  - Then applying **a horizontal shear**, we obtain:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110082649882.png)

- Rounding $sj$ to **the nearest integer** 
  - Ammounts to **taking each row** in the *image* and moving **sideways** by *some ammount*, which is **different** for *each row*
    - As its the **same displacement** in a row $\to$ enables us to **rotate** with *no gaps* in the **resulting image**
      - Similar idea for **vertical shear**
        - Thus implementing **simple raster rotation easily**

## 3D Linear transformations

- The **linear 3D transformation** is an *extension* of the **2D transforms**
  - Example $\to$ *scale* along the **cartesian axes**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110083014784.png)

- **Rotation** becomes *more complex* in **3D** to **2D** via increased **axes of rotation**
  - To **rotate** about the $z$ axis $\to$ only changes $x$ and $y$ 
    - Use **2D rotation** with *no operation* on $z$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110083138960.png)

- Can **construct matrices** to rotate about $x$ and $y$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110083202855.png)

https://www.youtube.com/watch?v=yF8cKSIw6l4& - derivation

- Discuss **arbitrary axes** rotation in the **next section**
  - For **2D dimensions**
    - We can **shear** along **some particular axis**
      - Example:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110084438356.png)

- As for **2D transforms** $\to$ any **3D** *transformation matrix* may be **decomposed** via **SVD** to a **rotation , scale** and *another rotation* 
  - Any **symmetric** *3D* *matrix* has an **eigenvalue decomposition** into *rotation / scale / inverse rotation*
    - Finally a **3D rotation** can be **decomposed** to a **product** of **shear matrices**

### Arbitrary 3D rotations

- As for *2D* $\to$ the *3D* rotations are **orthogonal matrices**
  - This means *geometrically* that the **three rows** of the **matrix** are **cartesian coordinates** of *three mutually orthogonal* **unit vectors**
    - There are an **infinite number** of *such rotation matrices*
      - Write down **such  a matrix**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110085453988.png)

- $u = x_ux + y_uy + z_uz$
  - So on for $v, w$. 
    - The **three vectors** are **orthonormal** we *know that*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110085605840.png)

- Can *infer* some of the **behaviour** of the **rotation matrix** 
  - Via applying to the **vectors** $u , v   ,w$ for example:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110090147851.png)

- Note that **these three rows** of $R_{uvw}u$ are **all dot products**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110090358439.png)

- Similarly $R_{uvw}v = y$, $R_{uvw}w = z$ 
- Thus $R_{uvw}$ takes the **basis** $uvw$ to the **coressponding** *cartesian axes* via **rotation**.

---

- If $R_{uvw}$ is a **rotation matrix** with *orthonormal rows*
  - Then $R^T_{uvw}$ is *also* a **rotation matrix** with *orthonormal columns*
    - It is the **inverse** of $R_{uvw}$ (*inverse* of **orthogonal matrix** is *always its transpose*)

---

- Important point is that **for transformation matrices** 
  - The **algebraic inverse** is *also* the **geometric inverse**
    - So if  $R_{uvw}$ takes $u$ to $x$ $\to$ then $\to$ $R^T_{uvw}$ takes $x$ to $u$ 
      - Same is **true** of $v$ and $y$ **confirming here**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110091451437.png)

- So we **always can create** *rotation matrices* from *orthonormal bases* 
  - If we **wish** to **rotate** about some *arbitrary* **vector** $a$ 
    - Can form some **orthonormal basis** with $w = a$ 
      - *rotate* that **basis** to the **canonical basis** $xyz$ 
      - *rotate* about the $z$ **axis**
      - *rotate* the **canonical basis** back to the $uvw$ basis

- In **matrix form** to *rotate* about the $w$ **axis** by an angle $\phi$:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110091821931.png)

- Have a $w$ **unit vector** in the *direction* of $a$ (*divided* by its **own length**)
  - What are $u$ and $v$ 
    - method to locate **reasonable** $u$ and $v$ given in **2.4.6** (constructing **basis** from **single vector**)

---

- If we have some **rotation matrix** 
  - And we **wish** to have the **rotation** in the **axis angle** *form*
    - Computer the **one real eigenvalue** ($\lambda = 1$) 
    - Along with the **corresponding eigenvector** which is the **axis** of **rotation**
    - This *axis* is **not changed** by **the rotation**

### Transforming Normal vectors

- Often **3D vectors** represent **positions** (*offset* from the **origin**) or *directions*
  - Such as **where light** comes from.
- Some *vectors* represent **==surface normals==** 
  - These are **perpendicular** to the **tangent plane** of a *surface* 
    - These *normals* do **not transform** the *way we would like* when the **underlying surface** is **transformed**.

---

- Example $\to$ if the **points** of a **surface** are **transformed** by some matrix $M$ 
  - A vector $t$ that is **tangent** to the **surface** and **multiplied** by $M$ is **tangent** to the **transformed surface**
    - However a **surface normal** *vector* $n$ transformed by $M$ may **not be normal** to the **transformed surface**.

---

- Can *derive* a **transform matrix** $N$ which **does not** take $n$ to a **vector** that is **perpendicular** to the **transformed surface**
  - One method of **attacking** this **issue**
    - Note that the **surface normal vector** and a **tangent vector** are **perpendicular** 
      - Thus $\to$ the **dot product** is $0$ , expressed in **matrix form** as:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110094019351.png)

- Denote the **desired transformed vectors** as $t_M = Mt$ and $n_N = Nn$ 
  - Goal is to find some $N$ such that $n^T_Nt_M = 0$ 
    - Can find $N$ by some **algebraic tricks**.
- Sneak some **identity matrix** into the **dot product**
  - Take **advantage** of $M^{-1}M = I$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110094532024.png)

- This **does not get anywhere yet**
  - Can add **parentheses** that make the **above** *expression* more **obviously** a **dot product**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110094642085.png)

- Therefore the **row vector** that is **perpendicular** to $t_M$ is the **left part** of this **expression**.
  - This expression **holds** for *any* of the **tangent vectors** within the **tangent plane**
- There is only **one direction** in **3D** (*and its opposite*)
  - That is **perpendicular** to *all tangent vectors*
    - We know that **left part** of the **expression** above **must** be the **row vector** *expression* for $n_N$ 
      - i.e. $\to$ it is $n^T_N$ , thus allowing us to **infer** the value of $N$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110095055812.png)

- Thus take the **tranpose** to obtain:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110095248232.png)

- See that the **matrix** which *correctly* **transforms** the *normal vectors* so they **remain normal** is $N=(M^{-1})^Tn$

  - This is the **transpose** of the **inverse matrix**

    - This **matrix** may *change* the **length** of $n$ , thus multiply by **arbitrary scalar** and still procuces $n_N$ with the **right direction**

      - The **inverse** of a **matrix** is the **transpose** of the **cofactor matrix** *divided* by the **determinant**
        - We do **not care** about the **length** of *normal vector* therefore **skip division** and find for a $3 \times 3$ matrix:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110095936157.png)

- Assumes the **element** of $M$ in row $i$ and column $j$ is $m_{ij}$ 
  - Therefore the **full expression** for $N$ is:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110100103298.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110100120319.png)

## Translations and Affine Transformations

- We have used methods to **change vectors** via a matrix $M$ 
  - For **2D dimensions** $\to$ these transforms have the **form**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110101011602.png)

- We **cannot** use **such transforms** to *move* objects
  - Only to **scale / rotate** them
    - The *origin* $(0,0)$ always remains **fixed** under a **linear transformation**
- To *move*  / or *translate* an **object** by **shifting** all its points the *same amount*
  -  We require **transform** of the **form**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110101206919.png)

- There is **no method** to do this via **multiplying** $(x,y)$ by a $2 \times 2$ matrix
  - A **possibility** for *adding translation* to the **linear transformation** system 
    - Is to **associate** a *seperate translation* vector with **each transformation matrix** 
      - letting the **matrix** *take care* of *scaling / rotation* and the **vector** take care of **translation** 
- *Feasible*
  - But as **simple and clean** with **multiple combined transformations** 
    - Use a **clever trick** to obtain a **single matrix multiplication** and do **both operations together**
- Simple idea 
  - Represent some *point* $(x,y)$ via a **3D vector** $[x \  y  \ 1]^T$ 
    - Use $3 \times 3$ matrices of **the form**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110101743120.png)

- The **fixed third row** is to *copy* the $1$ into the **transformed vector**
  - So every vector that have a $1$ in the **final place**
  - And the **first two rows** compute $x'$ and $y'$ as **linear combinations** of $x,y$ and $1$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110101920001.png)

- This **single matrix** implements a **linear transformation** followed by **translation**
  - This is an **==affine transformation==** 
    - The method of **implementing them** via adding an **extra dimension** is **==homogenous coordinates==** 
- **Homogenous coordinates** will *clean the code* for **transformations**
  -  It makes it **obvious** also how to **compose** *two affine transformations*
    - Just *multiplying* the **matrices**. 

---

- Issue with this **new formalisms**
  - Is when we must **transform vectors** that are **not supposed to be positions** 
    - They represent *directions* instead or *offsets* between positions.
- Vectors that **represent** the **directions** or **offsets** should **not change** when *translating an object*.
  - Arrange for this by **setting** the **third coordinate** to the value of *zero*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110102708380.png)

- If there is some **scaling / rotation** *transformation* in the **upper left** $2 \times 2$ entries
  - It **applies to the vector**
    - However the **translation** still *multiplies* with the **$0$** and *ignored* 
- The **$0$** is **copied** to the **transformed vector** 
  - Therefore the **direction vectors** *remain* as **direction vectors** after the **transformation**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110103024454.png)

- This is *precisely* the **behaviour** we require for *vectors*
  - Therefore they **fit smoothly** into the **system**
    - This *third coordinate* is either a $1$ or $0$ depending on whether we **encode** a **position / direction** 
- We should *store* this **homogenous coordinate** to *distinguish* between the **locations** and **other vectors**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110103651150.png)

- For *perspective viewing* $\to$ useful to enable this **final coordinate** to **change** to other values than zero.
- **Homogenous** coordinates are fundamental to all of the **graphics systems** and **hardware**
  - Enable **easy** *perspective viewing* 
- They are a *clever way* to **handle** the **bookkeeping** for **translation**
  - A *seperate* **geometric interpretation**:
    - The *key observation* when we do a **3D** *shear* based on the $z$ coordinate
      - We obtain **this transform**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110103937338.png)

- Almost the **correct form** of **2D translation**
  - But there is some **lingering** $z$ that does **not have meaning** in **2D**
    - Key decision:
      - Add a **coordinate** $z = 1$ to **every 2D location**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110104107784.png)

- Association of $z=1$ with **every** **2D point**
  - Can now **encode** *translations* to **matrix form**
    - Example $\to$ to **translate** in $2D$ by $(x_t, y_t)$ $\to$ **rotate** by angle $\phi$ 
      - Use the **following matrix**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110104239789.png)

- Note the **2D rotation matrix** is now a $3 \times 3$ with **zeros** in the *translation slots*
  - With this type of **formalism** using **shears** along the $z=1$ to **encode translations**
    - Represent *any* number of **2D shears**, **2D rotations** and **2D translations** as a **single composite 3D matrix**.
      - The *bottom row* of that matrix is always $(0,0,1)$ therefore we **dont really store it**.

- Just need to remember there **its there** when we **multiply two matrices** 
  - For **3D** $\to$ the *same technique works* 
    - Adding a **fourth coordinate** (*homogenous coordinate*) gives us **translations**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110122813859.png)

- For a **direction vector**
  - This *fourth coordinate* becomes $0$ and the **vector** is therefore **unaffected** by **translations**.

##### Example - Windowing transformations

- Often in **graphics**
  - We wish to **create** a **translation** from **matrix** that takes points in the rectangle $[x_l,x_h] \cross [y_l,y_h]$ $\to$ $[x_l^{'},x_h^{'}] \cross [y_l^{'},y_h^{'}]$
    - Accomplish via a **single scale** and *translate* in **sequence**
      - More *intuitive* to **create** the **transform** from a *sequence* of **three operations**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110123805102.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110123830049.png)

- Remember that the **right hand matrix** is *applied first*
  - We are able to write:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110123909616.png)

- Perhaps **not surprising** to some that the **resulting matrix** has this **current form** 
  - The **constructive process** with the **three matrices** leaves *no doubt* in the **correctness** of *said result*.

---

- An *analogous* construction can be used **to define** a **3D** *windowing transformation*
  - Maps the *box* $[x_l,x_h] \cross [y_l,y_h] \cross [z_l, z_h]$ , to the box:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110142852903.png)

- **Interesting** to *note* that if we **multiply** an *arbitrary matrix* composed of **scales / shears / rotations**
  - With some **simple translation** that comes after
    - We **obtain**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110143224867.png)

- Therefore we **can view** any *matrix* and think of it as **scaling  / rotation** part with a **translation part**
  - As each *component* is **nicely separated** from *each other*
- Important class of **transforms** are **rigid body** transforms
  - These are **composed** of **translations / rotations**
    - Therefore have *no stretching / shrinking* 
      - Thus have **pure rotation** for $a_{ij}$ above.

## Inverse of Transformations matrices

- Can always **algebraically** *invert* a **matrix**
  - We may **use geometry** if we **know** what said **transform does**.
    - Example $\to$ *inverse* of $scale(s_x,s_y,s_z)$ is $scale(1/s_x,1/s_y,1/s_z)$
- The **inverse** of a **rotation** is the *same rotation* with **opposite angle sign**
- The **inverse** of a **translation** is a **translation** in the **opposite direction**

---

- Say we have a **series** of *matrices* $M = M_1M_2 \cdots M_n$ then $M^{-1} = M_{n}^{-1} \cdots M_2^{-1}M_1^{-1}$ 
- Certain **types** of **transformation matrices** are *simple to invert*
  - *Scales* $\to$ are **diagonal matrices** 
  - *Rotations* $\to$ are **orthogonal matrices** (*inverse* of an **orthogonal matrix** is the **transpose**) 
- This makes it **simple** to **invert rotations** and **rigid body transformations**
- A matrix with $[0 \ 0 \ 0 \ 1]$ in the **bottom row** has an **inverse** with $[0 \ 0 \ 0 \ 1]$ in the **bottom row too**

---

- We can use **SVD** to *invert* a **matrix too**
  - We know that *any matrix* can be **decomposed** into a *rotation* **times** a **scale** times a **rotation** (RSR^T) 
- **Inversion** of this process is **simple**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110144735222.png)

- Rules above:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110144747004.png)

## Coordinate transforms

- Previous **sections** use **trasnformation matrices** to *move points*
  - Also think of this as **changing** its **coordinate system** for how **this point** is *represented*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211110145007387.png)

- We view **two ways** in *visualising* a *movement*

  - Different context may **prefer** each **context**

    - A *driving game* $\to$ may have some *model* of the city and *model* of the car.
      -  If the *player* is **represented** with a *view* **out** the **windshield**
        - Then *objects* inside the car are **always drawn** in the *same place on the screen* with the *streets* and **building** appearing to *move backward*
- Every *frame* we apply a **transformation** to *these objects* that **moves** them **farther back** than on the **previous frame**.

- One way to **think about this operation** is that it **moves** the **buildings backward** 
- Alternative method of **thinking** $\to$ is they are **staying put** but the **coordinate system** in *which* we **want to draw them** which is **attached** to the **car** is *moving*

---

- For the **second interpretation** 
  - The **transformation is changing** the *coordinates* of the **city geometry**
    - Expressing **them** as **coordinates** in the **cars coordinate system**.
- Both *methods* lead to the **same matrix** that is *applied* to the **geometry outside the car**.
  - If the **game supports** an *overhead view* to show where **the car** is in **the city**. 
    - The **buildings** and **streets** must be **drawn** in *fixed positions* while the **car** needs to **move** from *frame > frame*.
- The **same two interpretations apply**
  - Think of the **changing transformation** as *moving the car* from the **canonical** position to **its current location** in the *world*. 
  - Alternatively, think of the **transformation** as *changing* the **coordinates** of the **car geometry**.
    - Which was previously expressed in terms of **a coordinate system** attached to **car**
      - That was **expressed** in a **coordinate system** fixed **relative to the city**

---

- The **change of coordinates** *interpretation* makes it **clear** that the **matrices** are used in **these two modes**
  - From **city to car** *coordinate change* $\to$ **car to city** *coordinate change* 
    - These are **inverses** of **one **.

---

- The idea of **changing coordinate systems** is *much like* the *idea* of **type conversions** in **programming**
  - Before we can **add a floating point number** to an **integer** must do a **conversion** to make the **types match**.
    - And *before* we can **draw** the *city* and *car together* we need to **convert city** to **car coordinates** or **car** to **city coordinates**
      - Dependent on the **needs**.

---

- With **==multiple coordinate systems==**
  - Its easy to **get confused** and end up with **objects** in the **wrong coordinates** 
    - Causing them to **show** in **unexpected places**.
      - Just use *systematically think* about it **transformations** to *reliably right*

---

- *Geometrically* $\to$ a **coordinate system** or *coordinate frame* 
  - Consists of an **origin** and a **basis** (in 3D this is a *set* of **three vectors**)
- *Orthonormal bases* are **convenient** that we often assume **frames** are *orthonormal unless* **otherwise**

---

- For a frame with **origin** $p$ and ==**basis**== $\{u,v,w\}$ 
  - The **coordinates** $(u,v,w)$ will **describe** the *point*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118184525692.png)

- When storing **these vectors** in the **computer** 
  - must be **represented** in terms of **some coordinate systems**.
- Must designate some **==global coordinates==**  (*world coordinates*) 
  - This is used for describing every **other coordinate systems**.
    - For our **city example** $\to$ may adopt the **street grid** and use the convention that $x$ *axis* points along **main street** and $y$ axis **points up** along **central avenue**
  - When we write the **origin** and **basis** of the **car frame** in terms of **these coordinates** its *clear* what we **mean**.

---

- For our **2D convention** we use the **point** $o$ for the **origin** and $x,y$ for the **right handed** *orthonormal basis* vectors $x$ and $y$. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118201941931.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118202054049.png)

- An **alternative coordinate system** might have an **origin** $e$ and *right handed* **orthornormal** basis vectors $u$ and $v$ 
  - Typically the **canonical data** $o,x,y$ are *not stored explicitly* 
    - They are the **frame of reference** for *other coordinate systems*
      - Write the **location** of $p$ as an **ordered pair** :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118202449198.png)

- Example in **diagram above** $(x_p,y_p) = (2.5, 0.9)$
  -  Note the **pair** $(x_p,y_p)$ implicitly **assumes** the *origin* $o$ 
- Can express $p$ in terms of **another equation**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118202659459.png)

- In **figure** $\to$ this has $(u_p,v_p) = (0.5, -0.7)$.
  - This origin $e$ is left as **an implicit part** of the **coordinate system** associated with $u$ and $v$ 
- Express **same relationship** via *matrix machinery*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118202851471.png)

- Assumes we have **point** $e$ and **vectors** $u,v$ stored in **canonical coordinates** ($x,y$)  
- This is just a **rotation** (involving $u$ and $v$)$\to$ followed by **a translation** (involving $e$) 
- Its **simple to write down** 
  -  Just place $u,v$ and $e$ into the **columns** of a **matrix** with the **usual** $[0 \ 0 \ 1]$

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118203557503.png)

- Call this the **==*frame*-to-canonical==**  $F2C$ *matrix* for the $(u,v)$ **frame**
  - Takes points in $(u,v)$ *frame* and converts to **same points** expressed in the **canonical frame**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118203707598.png)

- To go in the **opposite direction**
  - We have:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118203806140.png)

- This is a **translation** by **rotation** (inverse of orthogonal rotation is transpose)
  - They are the **inverses** of the **rotation** / **translation** for *frame to canonical* matrix
    - When **multiplied** together the produce the **inverse** of the **frame to canonical matrix** 
      - Called the *==**canonical to frame matrix**==* $C2F$

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118204010323.png)

- The $C2F$ takes **points** expressed in the **canonical frame** and *converts* to **same points** expressed in the $(u,v)$ **frame**.
  - We have **written** this matrix as the **inverse** of the $F2C$ as it **cannot be immediately** written via $e,u,v$ 
- All **coordinate systems** are **equivalent** *representations*
  - Just our **convention** of storing $x,y$ in **vectors** creates this **seemingly** *asymmetry*. 
- The $C2F$ can be **expressed** simply **in terms** of the $(u,v)$ *coordinates* of $o,x$ and $y$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118205127023.png)

- Idea is **analogous** in **3D** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118205158376.png)

- And:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118205551752.png)

