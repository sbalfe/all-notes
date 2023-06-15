#  Viewing

- Important idea on moving **3D** to **2D** 
  - This is a **viewing transformation**.
    - Important role in **==object order rendering==** where we need to locate the **image space location** of every *object in the scene*.
- Project **3D points** to **2D points** (*world space* to *image space*) 
  - Project any **point** on a given **pixels** *view ray* back to **that pixel** *position* in the **image space**
- By itself **this point projection** is just good for **wireframe renderings**
  - Where only *edges are drawn* 
- Just as a **ray tracer** needs to locate the **closest surface intersection** along *each viewing ray* 
- An **object renderer** for **displaying solid objects** must work out which of **many surfaces** drawn at **any given point** on the *screen* are *closest* and **display one**.
- This chapter assumes **wireframes** for **simple cases**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118214924822.png)

## Viewing Transforms

- The *viewing transformation* has a *job* of **mapping** the **3D locations**
  - Represented as $(x,y,z)$ in the **canonical coordinate system** 
    - To **coordinates** in the *image* expressed via **units** of **pixels**.
- Requires **various aspects**:
  1. Camera Position
  2. Camera Orientation
  3. Type of projection
  4. FOV
  5. Resolution of the image
- Break into **several simpler transformations**
  - This is done via **three transformations** most likely.
    1. The **==camera transformation==** or **eye transformation**
       - A *rigid body* transformation that **places** the **camera** at the *origin* in a **convenient** *orientation*
       - Dependent only on the **position / orientation** (*pose*) of the **camera**.
    2. The **==projective transformation==** 
       - Projects **points** from the **camera space** so all *visible points* are in range $-1$ to $1$ in $x,y$ 
       - Depends only on the **type of projection desired.**
    3. A **==viewport transformation==** or **windowing transformation**
       - Maps the **unit image rectangle** to the **desired rectangle** in **pixel coordinates**.
       - Depends only on the **size** and **position** of the **output image**.

---

- We must **name** each **space** we are in to **make it easier**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118221636919.png)

- The **camera transformation** converts **points** in *canonical coordinates* (*world space*)
  - To **camera coordinates** or *places* them to **camera space**
- The **projection transformation** moves points from *camera space* to the *canonical view volume*
- The **viewport transform** maps the *canonical view volume* to **screen space**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118221835386.png)

### Viewport Transformation

- Assume the **geometry** we wish for is the *canonical view volume*.
- We wish to view this with an **orthographic camera** looking in the $-z$ direction
- The **canonical view volume** is the *cube* containing all **3D points** with *cartesian coordinates* between $-1$ and $+1$

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118222033996.png)

- We *project*:

  - $x=-1$ to the **left side of the screen**

  - $x=+1$ to the **right side of the screen**
  - $y=-1$ to the **bottom of the screen**
  - $y=+1$ to the **top of the screen**

---

- Each pixel *owns* a **unit square** centered at **integer coordinates**
  - The **image boundaries** thus have *half unit overshoot* from the **pixel centers** 
    - The **smallest pixel center coordinates** are $(0,0)$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118222606418.png)

- If we are **drawing** into an **image** (*or window* or *screen*) that has $n_x$ by $n_y$ pixels.
- We **map** the *square* $[-1,1]^2$ to the **rectangle** $[-0.5, n_x - 0.5] \times [-0.5,n_y -0.5]$ 
- **Assume** that *all line segments* to be **drawn** are *completely inside* the **view volume**
  - *clipping later*.
- Since the **viewport transformation** *maps* one *axis aligned rectangle* to **another**
  - Its the **case** of the **windowing transformation** given in *chapter 6*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118223331146.png)

- This matrix **ignores** the $z$ coordinate of **points** in the **canonical view volume**
  - As the **distance** along some *projection direction* does **not affect** where that **point** *projects* in the **image**. 

- Before we **call** it the **viewport matrix**
  - Must add a **row / column** to *carry* the $z$ coordinate with **no change**
    - use $z$ later to perform **depth checking** to figure out **what surfaces** are in **front of one another**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211118223753156.png)

- So breaking this down we have $\large\frac{n_x}{2}$ which is the **width over 2**
  - This is how much we **scale the point** from **canonical coordinates**
    - This calculates the **plane** , therefore we **translate** also by $\large\frac{n_x-1}{2}$ to obtain the **correctly scaled point**
    - This translation is **to map** the different **origins**
- Think of the unit squares **origin** as being the point mapped to the **center** of the **screen** 
  - Following  $(0,0)$
  - Looking here the **translation** takes (-1,-1) to the **origin** which is **exactly what we want**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119001633508.png)

https://youtu.be/HV1ZGyiY5C8?t=875

### The Orthographic Projection Transformation

https://www.youtube.com/watch?v=U0_ONQQ5ZNM - first bit talks on orthographic projection

- Often **render geometry** in the *some region* of **space** other than **canonical view volume** 

---

- First step in **generalization** of the *view*
  - Keep the **view directions** and **orientation** fixed looking *along* $-z$ with $+y$ upwards.
- We allow **arbitrary rectangles** to be **viewed** 
  - Rather than **replacing** the *viewport matrix* 
    - We *augment* it by **multiplying** with **another matrix** on the *right* 

---

- Under these **constraints** 
  - The *view volume* is an **axis aligned box**
    - name the **coordinates** of the *sides* so the **view volume** is $[l,r] \times [b,t] \times [f,n]$ 
      - This is the **==orthographic view volume==** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119010157711.png)

- The **bounding planes** *as follows*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119010346099.png)

- This **vocabulary** assumes a *viewer* who is looking at the $-z$ axis with **head** pointing in the **y direction** 
  - This implies $n > f$ which $\to$ assume the **orthographic view** volume has $-z$ values then $z = n$ is the **==near plane==** 
    - This is *closer* to the **viewer** *iff* $n>f$ where $f$ is a **smaller** *number* than $n$ (*negative number* of a *larger absolute value* than $n$ ) $\to$ shown **above**
- The **transform** from **orthographic** view to **canonical view** is *another window transform*
  -  Therefore **sub** the **bounds** of the **orthographic** and **canonical view volumes** to **equation**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119012202435.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119012330805.png)

http://learnwebgl.brown37.net/08_projections/projections_ortho.html - explains the translation

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119153930991.png)

- To draw **3D line segments** in the *orthographic view volume*
  - Project into the screens $x$ and $y$ coordinate then ignore $z$ *coordinates*.
    - Do this via **combination** of the **orthographic matrix** and the **viewport transform**.
- The *program* we *multiply* the **matrices** together then **manipulate** as follows:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119154156662.png)

- The $z$ coordinate is now in $[-1,1]$
  - We *do not take advantage* of this yet, but when we **use** the $z$ buffer **algorithms** we *will*
- Code to *draw* many **3D lines** with endpoints $a_i$ and $b_i$ thus becomes **both simple** and **efficient**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119154439715.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119154459497.png)

### Camera transformation

- Should have the ability to **change** the **viewpoint** in **3D** and *look* in *any direction*
  - There are **multitude** of **conventions** for *specifying* the **viewer position / orientation**
    - We use the *following* one:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119154953981.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119155026180.png)

- The **==eye position==** is a *location* that the *eye* *sees from*

  - If you think of **graphics** as a **photographic process**
    - This is the *center of the lens*.

- The **==gaze direction==** 

  - The **vector** in the **direction** where the person is **looking**.

- The **==view up==** is any *vector* in the **plane** that **bisects** the *viewers head* into **right and left halves** 

  - Then **points to sky** for a *person standing on the ground*

---

- These vectors give us **with enough information** to setup a **coordinate system** with origin $e$ and a $uvw$ basis.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119155510233.png)

- Using the **construction** in **section 2.4.7**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119155757191.png)

- This is the **end** if **all points** we wished to **transform** are stored in **coordinates** of *origin* $e$ and basis **vectors** $u,v,w$ 
  - Shown **in** *figure 7.7* the **coordinates** of the **model** are *stored* in terms of **canonical** (*world*) *orientation* $o$ and $x,y,z$ **axes** 
- To **use the machinery** we *have already developed* 
  - We need to **convert** the **coordinates** of the **line segment** endpoints we wish to **draw** from $xyz$ into $uvw$ *coordinates*
- This **kind of transformation** was covered in **chapter 6** $\to$ **coordinate transforms**.
  - The *matrix* that **enacts** this **transformation** is the *canonical to basis matrix* of the **cameras coordinate frame**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119160840097.png)

- Think of the **same transformation** as moving $e$ to the **origin**
  - Then **aligning** $u,v,w$ to $x,y,z$ 
    - To **make** our **previously** $z$ *axis* *only viewing algorithm* work for **cameras** with any **location / orientation**
- Back to chapter 6, we take the **points** from **one plane** to **another**
  - Each **points** **axis** must be expressed in **3 values** which are **determined** by the **basis** we are **taking into it**
  
    - This in sense is the **inverse** of the **xy** as shown above
  
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119165251843.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119164756225.png)

- Must add this **camera transformation** to the **product** of the **viewport / orthographic matrix**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119165459843.png)

- Not much code is required when the **matrices** are **in place**.

## Projective Transformations

- Recall that the **viewpoint** is **positioned** at the *origin* and the **camera** is looking along the $z$ *axis*
  - The **key property** of *perspective* is that **size** of an **object** on the *screen* is **proportional** to $\large \frac{1}{z}$
    - For an **eye** at the **origin** looking at the **negative** $z$ axis.
- This can **be expressed** precisely in an **equation** for the **geometry**  in *figure below*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119170347917.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119170421756.png)

- $y$ is the **distance** of the **point** *along* the $y$ axis and $y_s$ is where the **point** should be **drawn** on the *screen.*

---

- We would **like** to use the **matrices** for **orthographic projection** to *draw* the **perspective images** 
  - Could just **multiply** *another matrix* into our **composite matrix** and use the *algorithm* we *already have*.
- A **transformation** where *one* of the *coordinates* of the **input vector** appears in the **denominator**
  - Can *not be achieved* by **affine transformations**.
    - We can *allow* for **division** with a *generalization* of the **homogenous coordinates** that we used for **affine transformations**.

---

- Have agreed to represent the **point** $(x,y,z)$ using the **homogenous vector** $[x \ y \ z \ 1]^T$ 
  - This extra **coordinate** $w$ is *always 1*.
    - This is ensured by **always** using $[0 \ 0 \ 0 \ 1]^T$ as the **fourth row** of the **affine transformation matrix**.
- Rather than *just* thinking of the $1$ as an **extra** piece to **coerce** said matrix multiplication and to **implement translation**
  -  Now define the **denominator** of the $x,y,z,$ *coordinates*.
    - The **homogenous vector** $[x \ y \ z \ w]^T$ represents the **point** $(x/w, y/w, z/w)$ 
- This makes **no difference** for $w=1$ 
  - But thus **enables** a **broader range of transformations** to be *implemented* if we **allow any values** in **bottom** row the $w$ takes on values **other** than $1$. 

---

- Concretely **linear transformations**, allow us to *compute expressions* like:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119172640618.png)

- And **affine  transformations** extend this to:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119172623555.png)

- Treating $w$ as the **denominator** further *expands* the **possibilities**
  - Allowing us **to compute functions** like:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119172720744.png)

- This could be ***linear rational function*** of $x,y,z$ 
  - But there is **an extra constraint** $\to$ the *denominators* are the **same** for **all coordinates** of the **transformed point **

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119174851729.png)

- Expressed in a **matrix transformation**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119174920069.png)

- Transformation like this is a *projective transformation* or a *homography*.

##### Example - homography

https://towardsdatascience.com/understanding-transformations-in-computer-vision-b001f49a9e61

- Derivation of the matrix used is shown in the notes its a simple solution of $Ah=0$ where 
- $h$ is the **values to solve**, thus we need $A$ computation of its **SVD** and the **first singular value** where its **zero** which is in the **final coordinate at h9** is and thus the **further right singular vector** is the **list of solutions**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119180642543.png)

- The matrix:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119175046234.png)

- Represents a **2D projective transformation** that *transforms* the **unit square** $([0,1] \times [0,1]) $ to **quadrilateral** shown above.
  - For instance $\to$ the **lower right corner** of the *square* at $(1,0)$ is represented by **homogenous vector** $[1 \ 0 \ 1]^T$
    - The **transform** as **follows**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119175633498.png)

- Represents by  **the point** $(\frac{1}{\frac{1}{3}}, \frac{0}{\frac{1}{3}})$ or $(3,0)$ 
  - Note that if we **use the matrix** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119180034768.png)

- Instead the **result** is $[3 \ 0 \ 1]^T$ which *also represents* $(3,0)$ 
  - Any **fact** any scalar $cM$ is **equivalent**
    - The *numerator / denominator* are **both scaled** by $c$ 
      - Which does **not change the result**.

---

- There is a **more elegant** way of *expressing* the *same idea*
  - avoids **treating** the $w$ coordinate *specially*. 
    - In this view a **3D projective transformation** is just a **4D linear transformation**.
      - With the **extra stipulation** that *all scalar multiples* of a **vector** refer to **same point**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119181043218.png)

- Symbol $\sim$  is **similar to** , in this **case** its **equivalent meaning**.
  - Means that the **two homogenous vectors** described the **same point in space**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119190905651.png)

##### Example - Homogenous coordinates

- For **1D** *homogenous coordinates* $\to$ in which we use **2 vectors** to represent *points* on the **real line**.
  - We could *represent* the point $(1.5)$ using the **homogenous** *vector* $[1.5 \ 1]^T$ or *any other point* on the *line* $x=1.5h$ in **homogenous space**
- In **2D homogenous coordinates**
  - In which we use **3 vectors** to *represent points* in the **plane**
    - Represent the **point** $(-1 \ 0.5)$ using **homogenous vector** $[-2; \ -1;2]^T$ 
      - Or **any other point** on the *line* $x= \alpha[-1 \ -0.5 \ 1]^T$ 
- Any **homogenous vector** on the *line* can be **mapped** to the **lines intersection** in the plane $w=1$ to **obtain** said **cartesian coordinates**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119192405262.png)

- Its okay to **transform homogenous vectors** as *many times as required* 
  - With **out worrying** about the **value** of the $w$ *coordinate* in *fact*
    - Its **fine** if the $w$ *coordinate* is **zero** at some **intermediate phase**
- Only when we **only require** the **cartesian coordinates** is that we need to have a **point normalized** equivalent of $w=1$ 
  - Which ammounts by **dividing** all the **coordinates** of $w$ 
    - Once done $\to$ we can **read** off the $(x,y,z)$ coordinates from the **first three components** of the **homogenous vector**.

## Perspective Projection

- The *mechanism* of **projective transformation** make it *simple* to **implement** the *division* by $z$ that is **required** for **perspective**.
  - For **2D** example shown at **projective transformations section** 
    - We can **implement** the **perspective projection** with a **matrix transformation**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119193702273.png)

- This **transforms** the **2D homogenous vector** $[y; \ z; \ 1]^T$ to the **1D homogenous vector** $[dy \ z]^T$ 
  - Which *represents* the **1D point** $(dy/z)$ 
    - As its **equivalent** to **1D homogenous vector** $[dy/z \ 1]^T$ 
      - This matches **equation above** $\large y_s = \frac{d}{z}y$ 

---

- For the **official** *perspective projection  matrix* in **3D**
  - Adopt the **standard convention** of a **camera** at the **origin** facing in the $-z$ direction.
    - Therefore the **distance** of  *point* $(-x,y,z)$ is $-z$ 
- As like **orthographic projection** 
  - We adopt the **notion** of *near and far planes* that **limit** the **range** of **distances** to be *seen*
- For this **context** $\to$ use the **near plane** as the *projection plane*
  - Therefore the **image plane** is $-n$ 
- Desired **mapping** is $y_s = (n/z)y$ , similar for $x$ 
  - Transformation implemented by **perspective matrix**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119194708245.png)

https://youtu.be/U0_ONQQ5ZNM?t=322 - watch to understand, essentially the n+f and -fn are the coefficients required to map z to between n and f but nonlinearly warping them inbetween, this means that further z values depth inf

- note the n scaling is applying the perspective formulae for the distance from the camera to the near plane.

- The **first , second** and **fourth rows** implement the **perspective equation**.
  - The **third row** as in the **orthographic / viewport matrices**
    - Brings the $z$ coordinate **along** to use **for hidden surface removal**.
- For **perspective projection**
  - The addition of a **non constant** *denominator* prevents from **preserving** the *value* of $z$ 
    - its **impossible** to keep $z$ from changing while getting $x$ and $y$ to do **what we need** them *to do* 
- We keep **z unchanged** for **points** on the **near / far** planes.

---

- There are **many matrices** that *could function* as **perspective matrices**
  - All of them are **nonlinearly** *distorting* the $z$ coordinate.
    - This is **specific matrix** has the **nice** *properties* shown below

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119202847948.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119202902590.png)

- It leaves points on $z = n$ plane **alone** 
  - Leaves the **points** on the $(z=f)$ plane whilst **squishing** them in $x$ and $y$ by **appropriate ammount**
    - The **effect** of the **matrix** on a point $(x,y,z)$ is:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119203312042.png)

- As **you** can see the $x,y$ are **scaled** and also **divided** by $z$ 
  - Because both $n$ and $z$ (*inside the view volume*) are **negative** 
    - There are **no flips** in $x$ and $y$ 
- Although **its not obvious** 
  - The **transform** preserves the **relative order** of $z$ values between $z=n$ and $z=f$ 
    - This enables us to do **depth ordering** after the **matrix is applied**

- We want to take the **inverse** of $P$ 
  - To bring a **screen coordinate** plus $z$ back to the **original space**
    - As me may to do for **picking**, inverse:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119210549005.png)

- Since **multiplying** a **homogenous vector** by a **scalar** does *not change its meaning*
  - The **same** is **true** for **matrices** that operate on **homogenous vectors**
    - So we can write the **inverse** matrix in a **nicer form** multiplying **via** $nf$ 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119211357138.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119211411976.png)

- In context of **orthographic projection** matrix $M_{\text{orth}}$ 
  - The **perspective matrix** just maps the **perspective view volume** of a *frustum*
    - To the **orthographic view** volume (*axis aligned box*) 
- Once **applied** we simply just **use the orthographic transform** to obtain our **canonical view volume** 
  - Thus all of the **orthographic machinery** still **applies** and that was **added** was the **w division** (in sense $z$ as $z$ is **copied** into $w$ with the 1) 
- Concatenating $P$ with $M_{\text{orth}}$ results in the **perspective projection matrix**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120002300812.png)

- One **issue** is 
  - How are $l,b,r,t$ **determined** for **perspective** 
    - They identify the **window** from which we **look** 
- Since the **perspective matrix** does **not change** the *values* of $x$ and $y$ on the $z=n$ plane
  - Specify them **on this plane** 

---

- To implement our **perspective matrix** into our **orthographic infrastructure**
  - Simply replace $M_{\text{orth}}$ with $M_{\text{per}}$ 
    - This inserts **perspective matrix** $P$ after the **camera matrix** $M_{\text{cam}}$ has been **applied** before the **orthographic projection**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120010203503.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120010215433.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120010222943.png)

- The **only change** other than the **additional matrix** is division by the **homogenous coordinate** $w$ 
  - Multiplied out the matrix $M_{per}$ looks like this:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120010403369.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120010414966.png)

## Properties of the perspective transform

- Important **property** is that it takes **lines** to **lines** and **planes to planes**
- Takes **line segments** in the **view volume** to **line segments** in **canonical volume**

---

- Consider some **line segment**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120011337000.png)

- When **transformed** by a $4 \times 4$ matrix $M$ 
  - It is a **point** with **possible** *varying* **homogenous coordinate**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120011435760.png)

- The **homogenized** 3D line segment, *figure 7.6* :

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120011512495.png)

- If equation above can be **rewritten** in a **form**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120011727357.png)

- Then all **homogenized** *points* lie on a **3D line** 
  - Brute **force manipulation** of *figure 7.6*
    - Yields such a **form**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120012109559.png)

- Turns out **that** **line segments** do *map* to **line segments** thus *preserving* the **ordering** of the **points**
  - They **do not get reordered** or **torn**.
    - The **by product** of the **transform** taking **line segments** to **line segments**
      - Is that the **edges** and **vertices** of a **triangle** to the **edges** and **vertices** of **another triangle**
        - Therefore it takes **triangles** to **triangles** and **planes** to **planes**.

## Field Of View

- We can **specify** any **window** using the $(l,r,b,t)$ and $n$ values
  - Sometimes we wish for a **simpler system** where we **look** at the **center of the window**
    - Implies a **constraint** that:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120013552624.png)

- Adding this **constraint** that **pixels** are **square**
  - Meaning there is **no distortion** of **shape** in said *image*
    - Then the **ratio** of $r$ to $t$ must be the **same** as the **ratio** of the **number of horizontal pixels**.
      - To the **number** of **vertical pixels**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120013746869.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120013756121.png)

- Once $n_x$ and $n_y$ are specified , as in **resolution** of our **viewport** $\to$ leaves us **one degree** of **freedom**
  - This degree is the **==field of view==** shown as $\theta$ 
    - Alternatively called the **vertical field of view** to *distinguish* it from the **angle** between the **left / right sides** 
    - Or from the **angle** between the **diagonal corners**

- From the **figure** we see that 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120014057433.png)

- If $n$ and $\theta$ are **specified**
  - Then we can derive $t$ and use code for **more general viewing system**
    - Sometimes $n$ is **hard coded** to a **reasonable value**, this is often  $1$ but is essentially the **distance** to the **near plane**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120015900354.png)

- So top and bottom can be **calculated** from this **value **
- Must consider the **aspect ratio** so calculate **one value** using the **formulae** above then to obtain **other value** as in **width / height** or **top bottom** of **viewport** we **multiply** by the **aspect ratio.**
- Giving us the **final form of the perspective projection matrix**
- Note that the **values** in top 2 locations of third row are now **zero** via our **constraint**

