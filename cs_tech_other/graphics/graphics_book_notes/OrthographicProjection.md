## Orthographic Projection Matirx

1. First step is to **establish the convention** of the **window transform** and **affine transformation** *decompostion*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119021247515.png)

2. From this diagram its in **2D** but can be *generalised* easy to **3D**.

   - First equations shown is the **matrices** used in the **diagram** above to calculate this **new rectangle**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119021323760.png)

   - Generalised to **3D**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119021226922.png)

3. So these define the **three operations** involved in **converting** from **one window / volume** to **another** 

4. Now we will bring into **this explanation** the idea of an **orthographic view volume** to explain the **matrix decomposition**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119010157711.png)

   - So **6 planes** are defined for **this volume**.  

     - The *left, right, top , bottom, near, far*. which can be used to **describe** each **vertex** as you see on that **diagram**.

   - Linking the **affine transformation diagram** to further break down the meaning of these planes.

     - So we notice that the **bottom left corner** $(l,b,n)$ in terms of **our affine transformation** is $(x_l,y_l,z_l)$ (noticing clearly that $l$ is **left** of the **cuboid**)
     - But more importantly this **allows** us to **link** our **orthographic volumes** vertices to the **affine transformation diagram**

5. So now using this **notation** and **understanding** we begin the **orthographic volume** *mapping* to **NDC space** 

   - Following the notion of the **windowing transformation** , the first *step* is to **project**  us to the **origin** of the **volume / space** coordinate system that we are **transforming to**

     - In our case this is the **center** of the **world** at $(0,0,0)$ to be in the **middle** where the **NDC space** with $[-1,1]^3$ is placed its center.

   - This **translation** involves first **calculation** of the **center point** in terms of **all 3 axis**

   - This can be **computed** by taking an **average** of the **right / left planes** as example for the $x$ 

     - Therefore $\large\frac{r+l}{2}$ , we perform the **exact same logic** for **y,z** 
     - The equivalent in affine transformations would be therefore $\large{\frac{x_l+x_h}{2}}$

   - This obtains us **three values** for the *center* of each one
     $$
     \text{average of the two sides of x: } \  \ \large\frac{r+l}{2} \\
     
       \text{average of the two sides of y: } \ \large\frac{t+b}{2} \\
     
       \text{average of the two sides of z: } \ \large\frac{n+f}{2} \\
     $$

   - We have this **value** but we must take the **negative** of this as to **move it** to $(0,0,0)$ 

   - This creates the **first key matrix** in our *derivation*

   <img src="https://cdn.discordapp.com/attachments/309758102582984705/911102178440134676/image0.jpg" alt="img" style="zoom:33%;" />

6. Following on from this we **now** are at the **center** of our **world space**

   - We perform a **scaling now** , so looking at this *specific section* of the **affine transformation**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119034240216.png)

   - So from the **graphical diagram** it takes the **difference vector** between the **corresponding diagonals** of the **space** we wish to **reach**
   - Notice further that as we are transforming to **NDC** , this **explicitly** tells us that the **distance** between the **right** and **left** planes is $2$ 
   - This $2$ is **divided** by our **original** *difference* in **distance**
   - This is **diagonally applied** to each **coordinate** which intuitively **scales** the **volumes** into **NDC space** 
   - Creating the second and final **key matrix**:

   <img src="https://cdn.discordapp.com/attachments/309758102582984705/911103009407262730/image0.jpg" alt="img" style="zoom: 50%;" />

7. The **final** step is to **now apply** a **translation** *within* the **basis** that we have **reached** at the **origin space**

   - This **translation** is actually **implicitly developed** from our **derivation** by **multiplying** the two *translation* matrices that we have derived to create the **final matrix** 
   - For example it creates an **addition** to the **translation part** as shown

   <img src="https://cdn.discordapp.com/attachments/309758102582984705/911103981332353074/image0.jpg" alt="img" style="zoom: 33%;" />

   - Notice this is the **final matrix** as we just place this in the last bit
   - Intuitively this part is just **moving** the **to the correct point** based on the **scale factor** of **this basis** as to **perform a double translation**. 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119012330805.png)

