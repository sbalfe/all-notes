# Graphics Pipeline

- Must now look at *object order rendering* (OOR)
  - Unlike *ray tracing* $\to$ where you consider **each pixel** in **turn** and find the **objects** that **influence** its color
    - We now **consider** every *geometric object* in  turn and find the **pixels** it *could have an effect on*.

---

- The process of **finding all the pixels** in an image that are **occupied** by some *geometric primitive* is **==rasterization==**
  - Thus **OOR** also called *rendering by rasterization*. 

---

- The **sequence of operations** that is required
  - Starting with *objects* and ending by **updating pixels** in the **image**
    - Known as the **==graphics pipeline==**.

---

- OOR is very *efficient*
  - For **large scenes**, *management* of *data access patterns* is **crucial to performance**
    - Then making a **single pass** over the scene visiting **each bit of geometry** once has **many advantages** over *repeatedly searching* the scene to **retrieve objects** required to **shade each pixel**.

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130214809147.png)

---

- The **hardware pipeline** determines the overall **support** of **interactive rendering**
  - For examples OpenGL and Direct3D.
- Hardware pipelines must run **fast enough** for **real time** use in **games  /  visualisations** and **user interfaces**.
- Production pipelines must render the **highest quality animation** and **visual effects** possible and **scale** to **enormous scenes**
  - They take **much more time to do so**.
- Lots of pipelines **share common features**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130215537801.png)

- Work that must be completed can be organized into the **task** of **rasterization itself**
  - Operations that are **done to geometry** *before rasterization*.
  - Operations that are **done to pixels** *after rasterization*.
- Most **common geometric operation** is *applying matrix transformations*
  - Mapping the points that **define** the **geometry** from **object space** to **screen space**
    - So that the **input** to the **rasterizer** is *expressed* in **pixel coordinates** or **screen space**.
- Most **common pixelwise** operation is **==hidden surface removal==** which arranges the **surfaces** closer to the **viewer** to appear in **front of the surfaces** *farther* from the **viewer**.
- Many additional **operations** can be added in **each stage**.
  - Thus enabling a **wide range** of *rendering effects* using the **same idea**.
- Describe here:
  1. Vertex processing
     - forms the vertices for rasterization
  2. rasterization
     - forms the primitives from the process
  3. fragments
     - forms the individual parts of the primitives
  4. fragment processing
     - apply values to each fragment for each pixel of the primitive
  5. fragment blending stage
     - combine fragments to form the final value.

---

## Rasterization

- For **every primitive** that comes in
  - The *rasterizer* has **2 jobs**
    1. It *enumerates* the **pixels** that are **covered** by the **primitive** 
    2. It then *interpolates values* called **attributes** across the **primitive** 
- Each **fragments** *lives* at some **particular pixel** and carries its **own set of attributes values**.
  - Here we *present rasterization* with a **view** toward using it for **3D rendering**
    - Same idea for **2D**
      - Makes sense for 3D use in 2D drawing in terms of **under the hood**

### Line drawing

- Takes **two endpoints** in *screen coordinates* and draws a **line between**
  - Example $\to$ call for **endpoints** $(1,1)$ and $(3,2)$ turn *on* **pixels** $(1,1)$ and $(3,2)$
    - Fill in **one pixel** between them.
- For *general screen coordinates* $\to$ $(x_0,y_0)$ and $(x_1,y_1)$ 
  - Routine should draw some *reasonable* set of **pixels** that **approximates** a *line between them*
    - Drawing such lines based on **line equations**
      - Choose between:
        1. Implicit
        2. Parametric

#### Line drawing using implicit Line equations

- Most common way to **draw** lines using **implicit equations**
  - Is the ***midpoint algorithm*** 
- Ends up drawing the same lines as the **bresenham algorithm**

https://en.wikipedia.org/wiki/Bresenham%27s_line_algorithm

- But its **more simple**

----

- First is to **find the implicit equation** for the **line discussed** in *section 2.5.2* 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130223219839.png)

- Assume that $x_0 \leq x_1$ 
  - If that is **not true**
    - We **swap the points** to make this true
- The *slope* $m$ of the **line is given by**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130224419882.png)

- Following discussion assumes $m\in (0,1]$ (bracket means not including 0)
  - Analogous **discussion** derived for :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130224738608.png)

- The **four cases** cover *every possibility*
  - First case considers every negative value not including horizontal lines
  - second case is the flipped negative analogue of the one we are considering
  - third case is the same as first case but for the positive gradients never horizontal.

---

- For the case $m \in (0,1]$ 
  - There is *more* **run** than **rise**.
    - Line moves *faster* in $x$ than for $y$. 
- If there is some **API** where the $y$ axis **points downward**.
  - We *might have a concern* about whether this **makes** the overall **process harder**
    - However we **may ignore this**
- Ignore ideas of **up / down**
  - The *algebra* is the **same**
    - Following algorithm is valid for **the** $y$ axis *downwards case*.
- The **key assumption** of the **midpoint algorithm** is that *we draw* the **thinnest line possible**
  - That contains **no gaps** 
    - A **diagonal connection** between **two pixels** is *not considered a gap*.

---

- As the **line progresses** from the *left endpoint* to the **right**
  - There are **only two possibilities** 
    1. Draw a pixel at the **same height** as the **pixel** to *drawn to its left*.
    2. Draw a pixel **one higher**.
- There is always **exactly one pixel** in *each column of pixels* between the **endpoints**.
  - Zero $\to$ **implies** some *gap is present*. 
  - Two $\to$ **too thick a line**.
- There may be **two pixels** in the *same row* for our **current case**
  - The line being **more horizontal** than **vertical** therefore *sometimes* it may **go right** and **sometimes up**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130231240655.png)

- There are **three reasonable lines** that have been shown
  - Each **advancing** more in the **horizontal** direction than in **the vertical direction**
- The **midpoint algorithm** for $m \in (0,1]$ first **establishes** the **leftmost pixel** and the **column number** $x$ *value* of the **rightmost pixel** and then **loops horizontally** 
  - Establishing the **row** $y$ *value* of **each pixel**.
    - The **basic form** of the **algorithm**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130232257314.png)

- Note that $x$ and $y$ are **integers**. 
  - This says 

> Keep drawing pixels from left to right and sometimes move up in the y direction while doing so

- The **key** is establishing **efficient methods** to make the **decision** in the **if statement**

  - An *effective* method is looking at the **midpoint** of the line between the **two potential centers**.

- Specifically $\to$ the pixel just drawn is $(x,y)$ whose center is in **real screen coordinates** $(x,y)$ 

  - The **candidate pixels** to be *drawn* to the *right* are $(x+1,y)$ and $(x+1,y+1)$ 
    - The **midpoint** of these two points is $(x+1, y + 0.5)$ 

  - If the **line passes** below the *midpoint* we **draw the bottom pixel** 
    - Otherwise the **top pixel**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130234617542.png)

  - To **decide** whether the **line passes** *above* or *below* $(x+1, y+0.5)$ 

    - We **evaluate** $f(x, y + 0.5)$ in *implicit line equation above* 

  - Recall from **mathematical fundamentals** that $f(x,y) = 0$ for points $(x,y)$ on the line

    - $f(x,y) > 0$ $\to$ points on **one side** of the **line** 
    - $f(x,y) < 0$ $\to$ points on the **other side**

  - $-f(x,y) = 0$ and $f(x,y) =0$ are **good equations** for the line

    -  Not clear whether $f(x,y)$ being **positive** indicates that $(x,y)$ is **above the line** or **below**

  - In the **implicit line equation**

    - The *key term* is $y$ term $(x_1 - x_0)y$ 
      - This is **definitely positive** as $x_1 > x_0$ 
        - This means as $y$ **increases** the term $(x_1 - x_0)y$ gets **larger** (*more positive* / *less negative*) 

  - Thus the case $f(x, +\infty)$ is **definitely positive** and **definitely above** the *line*

    - Implying **points** above the **line are all positive**.

  - Alternative view is that the $y$ component of the **gradient vector** is **positive**

    - Therefore *above the line* $\to$ where $y$ **increases arbitrarily** in $f(x,y)$ must be **positive**

      - This means we **make our code more specific** by *filling* in this **if statement**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211130234814568.png)
    
  - The **code above** $\to$ works nicely for **lines** of the **correct slope** 

    - between $0$ and $1$ 

    >  -- self notes -- 
    >
    > As we know **above the line** is positive values, if the point above the line  negative then
    >
    > - it must be below the line and thus we draw then point upwards thus increment the y value.

  > -- self notes summary --
  >
  > so essentially we must approximate a line 
  >
  > we move the line from one point to another and this covers discrete values over its movement from these 2 points
  >
  > this goes from some y_0 to y_1 which may be stay on the same value or go upwards
  >
  > this just described that we can have one value in each column for no gaps as its a line 
  >
  > it said we can have multiple values in rows as it can go horizontally
  >
  > to decide whether to move up the y row we chose a midpoint decision to know which pixel to choose as in right up right and up based on the gradient vector positive value x_1 - x_0 y is positive therefore moving upwards is always positive as x_1 > x_0 moving y upwards is positive
  >
  > we discovered above the line is positive and therefore if the midpoint between the 2 potential pixels is < 0 the line must be above this midpoint and thus increment the value.

​       

-  If **greater efficiency** is *required* 
  - Using some **incremental method** can help
    - An **incremental method** attempts to *make a loop more **efficient*** 
      - Via the **reusing** of *computations* from the **previous step**
- For the **midpoint algorithm**
  - The *main computation* is **evaluating** $f(x+1, y+ 0.5)$ 
    - Note $\to$ **inside the loop** , after the **first iteration**
      - We have either **already evaluated** $f(x-1,y+0.5)$ or $f(x -1, y-0.5)$ 
        - This is saying that we **drew blue already** so we must already either have **evaluated** two potential points

          > --- self notes ---
          >
          > -  Which illustrated on the diagram below, if the point was evaluated to be above the line then the top point was used to draw this pixel, if the point was below the line then the bottom point was used.
          > -  So we use this similarity to only do a small addition rather than recalculation.
          >

          

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201001936607.png)

- Note **this relationship also**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201001213864.png)

- Enables us to **write** an **incremental version** of the *code*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201001437666.png)

- Should **run faster** as it has **little extra setup** compared to the **non incremental version**
  - May have more **numeric errors**
    - As the evaluation of $f(x,y+0.5)$ 
      - may be composed of **many adds** for **long lines**
- Lines are **rarely** *longer* than a **few thousand pixels** this error is **unlikely to be critical**
- Longer setup cost but **faster loop execution** achieved from **storing** $(x_1 - x_0) + (y_0 - y_1)$ and $(y_0 - y_1)$ as variables
- Hope a **good compiler** does this step for us
  - But if the **code is critical**
    - Wise to **examine** the *results* of the **compilation to make sure**.

### Triangle Rasterization

- Draw some **2D triangle** with **2D points** $p_0 = (x_0, y_0)$ , $p_1 = (x_1,y_1)$  and $p_2 = (x_2, y_2)$ in **screen coordinates*
  - Similar to **line drawing**
    - Just as **with line drawing** $\to$ wish to **interpolate color** or **other properties** from **values** at the **vertices**
- This is simple with **barycentric coordinates**
  - Example $\to$ colors $c_0, c_1, c_2$
    - The **colour** at a **point** in **triangle** with **barycentric coordinates** $(\alpha, \beta, \gamma)$ :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201174228477.png)

- This is known as **==Gouraud interpolation==** 

---

- We often **rasterizing** *triangles* that *share vertices* and *edges*
  - This means $\to$ we **wish** to **rasterize** the **adjacent triangles** so there are **no holes**.
    - Could be done using the *midpoint algorithm* $\to$ to draw the **outline** of **each triangle** 
      - The **fill interior values**
    - This means that **adjacent triangles** draw the **same pixels** along **each edge**.

- If the **adjacent triangles** have **different colours**
  - The *image* depends on the **order** in which the **two triangles are drawn**.
- Most **common** method of **triangle rasterization** that *avoids* the **order problem** 
  - And **eliminates** the *holes* $\to$ use the **convention** that **pixels** are **drawn** *iff* the **centers** are **inside the triangle**
    - As in being **within the interval** of $(0,1)$ 
  - What must be done on the **edge of the triangle**?
    - Several methods discussed later.
- **Key observation** $\to$ *barycentric coordinates* enable us to **decided** whether to **draw a pixel** 
  - Alongside **what color that pixel** should be if we are **interpolating colors** from the **vertices*
- The **issue** of **rasterizing** the **triangle** comes to **efficiently** finding the **barycentric coordinates** of **pixel centers**

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201181219860.png)

---

- The **rest of the algorithm** *limits* the **outer loop** to some **smaller set of candidate pixels**
  - Then makes the **barycentric computation efficient**. 
- Add simple **efficiency** by finding the **bounding rectangle** of the **three vertices** and just **looping over this rectangle** for **candidate pixels** to *draw*
  - To **compute barycentric coordinates** using equation

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201182835231.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201182846143.png)

---

- Here $f_{i,j}$ is the **line** that is given by the **equation** 8.1 with **correct vertices**
  - where $i$ and $j$ are the points or **vertices** of the **triangle** the line goes through.
- Recall the calculation calculates the **ratio** or **signed distance** from the **various lines** through **each point** and the **point being evaluated** based on the **various 3 lines** that are **going through each vertex**.
- note $0$ = C, $2=$ B and $1$ = A

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201182954256.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211105155916515.png)

- We have **exchanged** the **test** $a \in (0,1)$ with $\alpha > 0$
  - Because if all of $\alpha , \beta ,\gamma$ are **positive**
    - We know they are **less than one** as $\alpha + \beta + \gamma = 1$. 
- Can just **compute** *two* only of course:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201190047841.png)

- Not clear whether this **saves computational speed**
  - Once the algorithm is **made incremental**
    - Which is **possible** via **line drawing algorithms**
- Each of the **computations** of $\alpha, \beta$ and $\gamma$ dose an **evaluation** of the form $f(x,y) = Ax+By+C$ 
  - In the **inner loop** $\to$ only $x$ changes $\to$ changes by **one**
    - Note $f(x + 1,y) = f(x,y) + A$ 
      - This is the **basis** of the **incremental algorithm**. 
  - In the **outer loop** $\to$ evaluation changes for $f(x,y)$ to $f(x,y+1)$
    - So a **similar efficiency can be reached**
  - Because $\alpha, \beta$ and $\gamma$ change by **constant increments** in the **loop**
    - So does the **color**
      - Therefore this can be **made incrementl**
- Example $\to$ the **red value** for *pixel* $(x + 1,y)$ *differs* from the **red value** for **pixel** $(x,y)$ 
  - By some **constant ammount** that may be **precomputed**
    - Example of a **triangle** with **color interpolation** shown Above.

#### Pixels on Triangle Edges

- What to do for **pixels** whose **centers** are **exactly** on the **edge of the triangle**
  - If a **pixel** is **exactly** on the **edge**
    - Also on the **edge** of the **adjacent triangle** if **there is one**.
- No *clear way* to **award** the **pixel** to **one or the other**
  - A **worse decision** is to *not draw the pixel* as there would be a **hole** between the **triangles**
    - If they are **trasparent** $\to$ results in **double coloring**.
- Must give this **pixel** to some triangle
  - Must be a **simple process**
    - Which **triangle** chosen **does not matter** as long as **its well defined**.
- One idea is *note* that any **off screen point** is definitely on **exactly one side** of the **shared edge**
  - And that is the **edge we draw**
- For **two non overlapping triangles**
  - The **vertices** that are *not* on the **edge** are on **opposite** sides of the **edge** from **each other**
  - Precisely **one** of these **vertices** is on the **same side** of the **edge** as the **off screen point**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201192506326.png)

- This is the **basis** of the **test**
  - The test if **numbers** $p$ and $q$ have the **same sign** can be implemented as the **test** $pq > 0$ 
    - This is **efficient in most environemnts** 
- This is **not perfect** as the *line* through the **edge** may also **go through the off screen point**
  - We have **massively reduce** the **number of problematic cases**
- The **off screen point used** is just an **arbitrary decision**
  - $(x,y) = (-1,-1)$ is a **good choice** as *any* other.
- Must add a **check** for the **case** of a **point exactly** on an **edge**.
  - We would like this **check** not to **be reached** for **common cases**
    - which are the **completely inside / outside tests**
      - Suggest:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201194804059.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201193118419.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201193127709.png)

> --- self notes --
>
> Idea in the check is that **point** we evaluate **multiplied** by the **off screen point** will be **specifically on one side**
>
> - Just use the **sign** $pq > 0$ where $q$ is the **off screen point** and $p$ is the **vertex** of **our triangle** 
>   - If **weights** are **above** $0$ 
> - Basically the idea with $kf(x,y) > 0$ to decide which side is **positive and negative**
> - greater than 0 check is saying if its = 0 then we do the edge function decision check otherwise not just for efficient

- May expect **above code** would work to **elimate holes** and **double draws** only if **we use exactly** the **same line equation** for **both triangles**
  - The **line equation** is the **same** only if the **two shared vertices** have the **same order** in the **draw call** for **each triangle**
    - Otherwise the **equation** might **flip** in **sign**
      - This could be a **problem depending** on **whether** the **compiler changes** the **order of operations**
- Therefore if a **robust implementation** is **required** $\to$ the **details** of the **compiler and artihmetic unit** may require to **be examined**. 
  - The **first four lines** in the *pseuodocode* must be carefully **coded** to handle cases where the **edge** *exactly* hits the **pixel center**.
- In Addition to being **amenable** to an **incremental implementation**
  - There are **several potential** *early exit points* 
    - Example $\to$ if there is no **need** to **compute** $\beta$ or $\gamma$ 
      - This may **result** in a **speed improvement** $\to$ *profiling is a good idea*. 
    - The **extra branches** may **reduce pipelineing** or **concurrency** and **may slow code**
  - As **always** $\to$ test **any attractive looking optimisations** if the code is a **critical section**.
- Another detail of the **code above**
  - Is that the **divisions** could be **divisions** by **zero** for **degenerate triangles**
    - Where $f_{\gamma} = 0$ 
      - Either the **floating point error conditions** should be **accounted** for *properly* or *another test is required*

### Clipping

- **Transforming** *primitives* $\to$ **screen space** and **rasterizing** does **not work by itself**
  - This is because **primitives** that are **outside the view volume**
    - Such as those **behind the eye** may end up **being rasterized**
      - Therefore leading to **incorrect results**.
- Consider **triangle** shown below:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201202854117.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201202839883.png)

- **Two vertices** are in the **view volume**
  - But the **third is behind the eye** 
    - The **projection transformation** maps this **vertex** to some **non sensical location** that is **behind the far plane**.
      - If this is **allowed** $\to$ the **triangle** is **rasterizied incorrectly** 
    - For this reason $\to$ **rasterization** must be **preceded** by a **clipping operation** that *removes parts* of **primitives** that *could extend* **behind the eye**.

---

- *Clipping* is a **common operation** within **graphics**
  - Required when **one geometric entity** *cuts* another
    - Example $\to$ if you **clip a triangle** against the plane $x=0$ 
      - The **plane cuts** the **triangle** into **two parts** if the *signs* of the $x$ coordinates of the **vertices** are **not all the same**.
- For **most applications** of *clipping*
  - The *portion* of the **triangle** on the **wrong side** of the **plane** is *discarded*. 
    - This **operation** for a *single plane* shown below:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201210804461.png)

- In **clipping** $\to$ to **prepare** for **rasterization**
  - The **wrong side** is the **side** that is **outisde the view volume**
    - It is **safe to clip** away **all geometry** that is *outside the view volume*.
      - Thus *clipping* against **all six faces of the volume**
        - Many systems manage to **get away** with only **clipping** against the **near plane**.

- Two **most common approaches** for *clipping implementation*
  1. In **world coordinates** $\to$ using the **six planes** that *bound* the *truncated viewing pyramid*
  2. In the **4D transformed space** before the **homogenous divide**.

---

- Either possibility  can be **effectively implementd** using this **approach** for *each triangle*:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201211443607.png)

#### Clipping before the transform (Option 1)

- What are the **six plane equations**
  - These equations are **equivalent** for **all triangles** that are *rendered* in the **single image**
    - We *do need to* **compute** them *very efficiently*

- For this reason $\to$ can just **invert** the **transform** 
  - Then apply to **eight vertices** of the **transformed view volume**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201212000398.png)

- The **plane equations** can be **inferred** from **here**
  - Alternatively we may use **vector geometry** 
    - To obtain the **planes** directly from the **viewing parameters**.

#### Clipping in Homogenous coordinates (Option 2)

- Often clipped in **homogenous coordinates** *before the divide*
  - Here the **view volume** is $4D$ 
    - And its bounded by **3D volumes** *hyperplanes* 
      - These are:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201212412443.png)

- These planes **are quite simple**
  - Therefore the **efficiency** is better than for **option 1**
    - Can still be **improved** by **transforming** the *view volume* $[l,r] \times [b,t] \times [f,n]$ $\to$ $[0,1]^3$ 

- Turns out that the **clipping plane** of the **triangles** is **not much more complicated** than in **3D**

#### Clipping against a plane

- No matter **which option we choose**
  - Must **clip against** some **plane**
    - Recall from **mathematical fundamentals** $\to$ that the **implicit equation** for a *plane* through some *point* $q$ with **normal** $n$:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201214644573.png)

- This is **often written**: 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201214739022.png)

- This **equation** not only **describes** a **3D plane**
  - Also describes a **line** in **2D** and **the volume** *analog* of a plane in **4D**.
    - All of these **entities** are called **planes** in the **appropriate dimension**. 
- If we have a **line segment** between points $a$ and $b$ 
  - We can **clip** it **against** a *plane* using the **techniques** for **cutting** edges of **3D triangles** $\to$ in **BSP tree** programs
    - Described in a **later section**.

---

- The points $a$ and $b$ are **tested** to **determine** whether they are **on opposite** sides of the **plane** $f(p) =0$. 
  - By *checking* wheter $f(a)$ and $f(b)$ have **different signs**
- Typically $f(p) < 0$ is defined **inside the plane**
  - And $f(p) > 0$ is *outside* the **plane** 
    - If the **plane** does *split the line*
      - We can solve for the **intersection point** by **substituting** the **equation** for the **parametric line**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201222449694.png)

- Into the $f(p) = 0$ plane of **equation 8.2**
  - This yields:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201224016188.png)

- Solving for $t$ gives:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211201225102717.png)

- Then find the **intersection point** and *shorten the line*
  - To **clip a triangle** 
    - We again may **follow** section 12 to **produce one or two triangles**.

## Operations Before and After Rasterization

- Before some **primitive** is **rasterized**
  - The *vertices* that *define it* must be **in screen coordinates**
    - Alongside the *colors / other attributes* that are **supposed** to be **interpolated** across the **primitive** must *be known*.
- Preparing this **data** $\to$ is the *job* of the **==vertex processing stage==** of the **pipeline**.
  - The *incoming vertices* $\to$ are **transformed** by the *modelling / viewing / projection transformations*
    - Mapping from the **original coordinates** $\to$ **screen space** (where *position* is measured in **terms of pixels**)
- Information such as **colors , surface normals , texture coordinates** is *transformed as required*
- After **rasterization** $\to$ *further processing* is done to **compute a color** and **depth** for **each fragment**
  - This **processing** may be *as simple* as passing it **through** some **interpolated colour** and using **depth** *computed* by the **rasterizer**
    - Or Can involve **complex shading operations**

- The **==blending phase==** combines the **fragments** generated by the **primitive** (*possibly several*)
  - That **overlapped** *each pixel* $\to$ compute the **final color**
    - The **most common blending approach** $\to$ choose the **color** of the **fragment** with the **smallest depth** (*closest to the eye*)
      - The **purpose** of the **different stages** are *shown in examples:*

### Simple 2D Drawing

- The most **basic** *pipeline* does **nothing** in the *vertex / fragment stages* 
  - And the **blending** stage of **each fragment** just *overwrites* the **value** of the **previous one**.
- The **application** supplies **primitives** directly in **pixel coordinates**
  - Then the **rasterizer** does **all the work**. 
- The **basic arrangement** is the *essence* for **simple older APIs** for drawing **user interfaces / plots / graphs** and other **2D content**.
- Solid **color shapes** $\to$ can *be drawn* by **specifying** the **same color** for **all vertices of each primitive** and our **model pipeline** also *supports* **smoothly varying** *color* using **interpolation**.

### Minimal 3D Pipeline

- To **draw objects** in **3D** 
  - The only change **is** to **2D drawing pipeline** is a **matrix transformation**
    - The *vertex processing stage* $\to$ multiplies the **incoming vertex positions** by the **product** of *modelling / camera / projection / viewport matrices*
      - Results in the **screen space triangles** that are then **drawn** in the *same way* as if they were **directly specified** in **2D**.
- Problem with the **minimal 3D pipeline**
  - Is regarding to the **order** to obtain the **occlusion relationships** *correct* 
    - Obtain **nearer objects** that are **infront of further objects**.
      - This is **==painters algorithm==** for the **hidden surface removal**.
- Its a **valid method** to **remove hidden surfaces**
  - Has many **drawbacks**
    - It **cannot handle triangles** that **intersect one another**.
      - Because there is **no correct method** in which to **order them**.
    - Similarly $\to$ **several triangles** even if *they dont intersect*
      - Can be **arranged** in the **occlusions cycle**
        - Another **case** in which the **back to front** order *does not exist*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203192444557.png)

- Most **importantly** is *sorting* the **primitives** by **depth** is **slow**
  - Particularly for **large scenes**
    - This **disturbs** the *efficient flow* of **data** that **makes object rendering so fast**
- Figure below shows the **result** of this **process** when the **objects** are **not sorted by depth**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203192737824.png)

### Using a z buffer for hidden surfaces

- No one **really uses the painters algorithm**
  - Instead using a **simple / effective** *surface removal* algorithm called **==z-buffer==** **algorithm** is *used*.

- The **method** is *very simple*
  1. For **each pixel** $\to$ we **keep track** of the **distance** to the **closest surface** *drawn so far*
  2. **Throw** away the **fragments** that are *farther away* than *that distance*.
- The **closest distance** $\to$ stored by **allocating** an **extra value** for *each pixel* 
  - In **addition** to the **red , green** and **blue color values**
    - Which is **known** as that **depth** or **z value**.
- The **==depth buffer==** or **z buffer** is the **name** for the **grid** of **depth values**

---

- The **z buffer algorithm** is *implemented* in the **fragment blending phases**

  - It **compares** the **depth of each fragment** with the *current value* in the **z buffer**

    - If the **fragments depth** is *closer* $\to$ both the **color** and its **depth** value **overwrite** the **values** that are *currently* in the **color / depth buffers**.

    - If the **fragments depth** is *farther* $\to$ its **discarded**.

  - To **ensure** the *first fragment* will **path the depth test**

    - The **z buffer** is **initialized** $\to$ to the **maximum depth** (*the far plane* depth).

  - Irrespective of the **orders** in **which surfaces are drawn** $\to$ the **same fragment** will **win the depth test** and the **image is the same**.

- The **z buffer algorithm** $\to$ requires *each fragment* to hold a **depth**

  - This can be done **by interpolating** the $z$ coordinate as **vertex attribute** in the *same way* that some **color / other attributes** are **interpolated**.

- The **z buffer** is **such a simple** and **practical** way to **deal with hidden surfaces** in **object order rendering** that is the **most dominant approach**

  - Must *simple* than **geometric models** that **cut surfaces** into **pieces** that may be **sorted by depth**
    - Avoids this **convoluted issue**.

- The **depth order** must be **determined** at the **locations of the pixels**

  - That is **all the z buffer does**
    - Its *universally supported* by the **hardware graphics pipelines** and is **most commonly** used method for **software pipelines**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203194723785.png)

---

#### Precision Issues

- In **practice** $\to$ the $z \text{ values}$ stored in the **buffer** $\to$ are **nonnegative integers**. 
  - This is **in preference** over the **true floats** because the **fast memory** required for the **z buffer** is **expensive** and must be *kept to a minimum*.

- Using **integers** may cause **precision issues**
  - Using an **integer** can cause **precision problems** 
    - Using an **integer range** having $B$ values $\{0,1, \cdots ,B -1\}$ 
      - Can map $0$ to the **near clipping plane** $z= n$ and $B-1$ *far clipping plane* $z=f$ 

- Assume that $z,n,f$ are **positive**
  - This *results* in the **same results** as the **negative case**
    - But the *details* of the **argument** are easier to **follow**

---

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203200324317.png)

- Send each $z$ *value* to some **bucket** with *depth* $\Delta z = (f-n)/B$ 
  - We would **not** use the **integer z buffer** if *memory* were *not a premium*
    - Thus **useful** to make $B$ as **small as possible**.
- If we *allocate* $b$ bits to store the **z value** then $B = 2^b$ 
  - Need **enough bits** to ensure that **any triangle** in *front* of **another triangle** have its **depth** mapped to **distinct bins**.
- Example $\to$ if you are **rendering a scene** where **triangles** have a *separation* of at **least one meter**
  - Then $\Delta z < 1$ should give **images** that pertain **no artifacts**.
- There are **two ways** to make $\Delta z$ **smaller**
  1. Move $n$ and $f$ **closer together**
  2. *Increase* $b$.
- If $b$ is **fixed** 
  - Adjusting $n$ and $f$ is the **only option** 
- The **precision** of **z buffers** must be **handled** with **great care** when **perspective** images are **created**
  - The *value* $\Delta z$ above is used *after* the **perspective divide**.
    - Recall from **previous section 7.3** that the **result** of the **perspective divide**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203201311027.png)

- Actual **bin depth** is related to $z_w$ $\to$ the **world depth** rather than $z$ 
  - The **post perspective divide depth**
    - We may **approximate** the **bin size** by *differentiating both sides.* (quotient rule and product rule)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203201609538.png)

- Bin **sizes** vary in **depth**
  - The **bin size in world space** is:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203201743509.png)

- Note the **quantity** $\Delta z$ discussed **earlier**
  - The **largest bin** will be for $z' =f$ , where:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211203201900025.png)

- Choosing $n=0$ 
  - A **natural choice** if we dont want to **lose objects** that are **right in front of us**
    - This results in an **infinitely large bin** $\to$ a *very bad condition*.
- To a make $\Delta_{z_w}^{\text{max}}$ as **small as possible** 
  - Must **minimize** $f$ and **maximise** $n$ 
    - Therefore its **always important** to *choose* $n$ and $f$ carefully.

### Per Vertex Shading

- So far the **application** *sending triangles* into the **pipeline** is *responsible* for **setting color**
  - The **rasterizer** just **interpolates** the **color** and they are *written* directly **into** the **output image**.
- For **some applications** $\to$ this *is sufficient*
  - Many cases $\to$ we want $3D$ objects to be **drawn** with **shading**
    - Using the **same illumination equations** (*chapter 4*)
- Recall these require **light direction / eye direction / surface normal** to *compute* the *color of a surface*

---

- One method to **handle** *shading computations* is to **perform** them in the **vertex stage**
  - Application provides **normal vectors** at the **vertices** , and **the positions** and **colors** of the **lights** are *provided seperately* (They dont vary **across the surface** therefore they *dont need to be specified for each vertex*)
- For **each vertex** $\to$ the *direction* to the **viewer** and the **direction** to *each light* are **computed** based on **positions** of the *camera / lights / vertex*
- The **desired shading** $\to$ equation is *evaluated* to **compute the color**
  - Then passed to **rasterizer** as the **vertex color**
    - This **per vertex shading** is referred to as **==Gouraud shading==**.

---

- A **decision** to be made is the **coordinate system** where the **computations done**
  - *World / Eye* space are **good choice**
- Vital to select a **coordinate system** that is **orthonormal** when viewed in **world space**
  - This is because **shading equations** depend on **angles between vectors**
    - Which are **not preserved** like *non uniform scale* that are **often used** in the **modelling transformation** or **perspective projection**.
      - Often used in the **projection** to the **canonical view volume**.

---

- *Shading* within the **eye space** has the **advantage** that we *dont keep track* of the **camera position**
  - As its *always* at the **origin** of the **eye space** 
- In **perspective projection** $\to$ or the **view direction** is *always* $+z$ in **orthographic projection**

---

- **Per vertex shading** has a *disadvantage* that it **cannot produce** *any details* in the **shading** that are **smaller** than the **primitives** used in **drawing the surface**
  - This is as it **computes** *shading* just **once** for *each vertex* and **never between vertices**

- For **instance** $\to$ room with a **floor** that is **drawn** using **two large triangles** and **illuminated** by a **light source** in the **middle of the room**
  - Shading will be **evaluated** only at the **corners of the room** 
    - The **interpolated value** will *likely* be **much too dark** within the **center**
- Also **curved surfaces** $\to$ that are **shaded** with *specular highlights* 
  - Must be **drawn** $\to$ using **primitives** *small enough* that the **highlights** can be *resolved*

- Figure below shows two spheres using **per vertex shading**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211204211410679.png)

### Per Fragment Shading

- To **avoid** the **interpolation** associated in **per vertex shading**
  - Can *avoid* **interpolation** of **colors** by *performing* the **shading computations** *after* the **interpolation**
    - Within the **fragment stage**.
- For **==per fragment shading==**
  - The **same shading equations** are *evaluated*.
    - But they are **evaluated** for *each fragment* using the **interpolated vectors** 
      - Rather than **for each vertex** using the **vectors** from the **application**
  - In **per fragment shading** $\to$ the *geometric information needed* for **shading** 
    -  Passed **through the rasterizer** as **attributes**
      - So the **vertex stage** must *coordinate* with the **fragment stage** to *prepare* the *data appropriately*.
- One **approach** is to **interpolate** the *eye space* *surface normal* and the *eye space vertex position*
  - Then can be **used**  just as like in **per vertex shading** 
    - Figure below shows **per fragment shading**

### Texture Mapping

- *Textures* $\to$ are **images** that used to **add extra detail** to the *shading of surfaces*

  - That would otherwise look **homogenous / artificial** 
- Idea is **simple** $\to$ each time **shading is computed**

  - We **read one of the values**  used in the **shading computation** $\to$ the **==diffuse color==**

    - For instance $\to$ from a **texture** instead of using **attribute values** that are *attached* to the **geometry** being **rendered**

      - This is called **==texture lookup==**
- The **shading code** specifies some **texture coordinate** 
  - A **point** in the *domain of the texture*
    - Then the **texture mapping system** finds the **value** at that point in the **texture image** and **returns it**
- The **texture** value is then **used** in the **shading computation**
- The **most common way** to *define texture coordinates* is just to **make the texture coordinate** some *other vertex attribute*
  - Each **primitive** then **knows** where it **resides in the texture**.

### Shading Frequency

-  The **decision** about *where to place* each **shading computations**
  - Depends on **how fast** the *color changes*
    - The **scale** of the **details being computed**.
- Shading with **large scale features** $\to$ such as **diffuse shading**
  - On **curved surfaces** can be *evaluated fairly infrequently* and then **interpolated**.
    - Can be computed with a **low shading frequency** 
-  Shading that **produces** *small scale features* 
   - Such as *sharp highlights* or **detailed textures** must be **evaluated** at **high shading frequency** 
-  For details that require **sharp and crisp** in the image
   - Shading frequency needs to be **at least** *one shading sample per pixel*
   - As so **large scale effects** can *safely be computed* at the **fragment stage** when **primitives** are *larger than a pixel*

---

- Example $\to$ a **hardware pipeline** used in some *computer game*
  - Generally using **primitives** that *cover several pixels* to **ensure high efficiency**
    - Often does **most shading computations** *per fragment*.
- Alternatively $\to$ the **photorealistic** `RenderMan` does all **shading computations** *per vertex*
  - After first **subdividing / dicing** surfaces into **small quadrilaterals** called **==micro polygons==**  
    - Similar to the **size of pixels**
- Since the *primitives* are small, per vertex shading in this **system** achieves a **high shading frequency** suitable for **detailed shading**.

### Simple Antialiasing

- Just as with *ray tracing*
  - *rasterization* $\to$ produces **jagged lines** and **triangle edges** if you make an **all or nothing determination** of whether **each pixel** is **inside the primitive or not**. 
- The set of **fragments** generated by the basic **triangle rasterization algorithms** within this chapter
  - Referred to as **standard / aliased rasterization** 
    - Is the *same* as the **set of pixels** that are mapped to **that triangle** by a *ray tracer* that sends some **single ray** through **centers** of **each pixel**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211217220909762.png)

- Alongside with **ray tracing**
  - The *solution* is enabling pixels to be **partly covered** by a **primitive**
    - This form of *blurring* enables a **greater visual quality**
      - Especially for **animations**

---

- Just as with a **ray tracer**
  - Can produce an **antialiased image** by setting *each pixel value* to the **average color** of the **image** over the **square area**
    -  Belonging to the pixel.
      - Known as **==box filtering==**
- This means we think of *all drawable entities* having **well defined areas**.
  - The line above thought of as **approximating** a *one pixel wide rectangle*
    - Easiest method to **implement box filter antialiasing** is by **==supersampling==** 

---

- *Supersampling* creates image at very **high resolutions** and then **down samples**.
  - Example $\to$ goal is a $256 \times 256$ pixel image of  a line with $1.2$ pixels in width
    - We could *rasterize* a **rectangle version** of the *line* width $4.8$ pixels on a $1024 \times 1024$ screen
      - Then **average** $4 \times 4$ groups of **pixels** to get **colors** for each of the $256 \times 256$ for the **shrunken image**.

---

- This is an **approximation** of the *box filtered image* but works well **when objects** are *not extremely small relative* to the **distance between pixels**.

---

- *Supersampling* is **quite expensive**
  - Because the **sharp edges** that *cause aliasing* are often caused by **edges of primitives** 
    - Rather than **sudden variations** in *shading* within a **primitive**.
- 

