# Ray Tracing in One Weekend

https://github.com/RayTracing/InOneWeekend

##  Output an image

- Write some file
- Start with **ppm text file**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125232121956.png)

https://www.cs.rhodes.edu/welshc/COMP141_F16/ppmReader.html

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220125234517311.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126044815661.png)

## Vec3 Class

- Colors
- Locations
- Directions
- Offsets
- ....

---

- Uses floats, but in **some ray tracers** used doubles. 
- Everything is in the **header file**, and **later** on in the **file** there are **many vector operations**.

- change the main to use the vector class

## Chapter 3 Rays , simple camera and background

- Ray is some function $p(t) = A + t*B$ 
  - $p$ $\to$ some **position** in $3D$ along a **line** in $3D$ 
  - $A$ $\to$ the **ray origin** 
  - $B$ $\to$ the **ray direction**

- The ray *parameter* $t$ is a **real number**
- Plug in various $t$ and $p(t)$ moves the **point along the ray**
  - Additional $-t$ enables us to **go anywhere** on the **3D line**.
  - For $+t$ $\to$ you obtain only the **parts** in front of $A$
  - This is **what is often** called a **half-line** or **ray**.
- Example $C = p(2)$: 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126020932933.png)

---

- The function $p(t)$ in **more verbose** code form **I** call *point_at_parameter(t)* 

---

### Fundamental concept of ray tracing

- Ray tracers **send rays** *through pixels* and **compute** the *color seen* in the **direction** of *these rays*

> - Form *calculate* which ray **goes** from the **eye to the pixel**
> - **compute** what the **ray intersects**
>
> -  **compute** some *color* for this **intersection point**

- Develop some **basic camera** for getting the **code running**

- Obtain a **basic** *color / ray* function to **return** the *color of the background* (*simple gradient*)

---

- Place **==eye==** (*camera center*) at $(0,0,0)$ 
  - $y$ is *up* 
  - $x$ is *right* 
- **RHS** $\to$ convention of **right handed coordinate system**
  - into screen $\to$ $-z$ 
- We **traverse** the *screen* from the *lower left corner*
  - Use **two offset vectors** along screen sides to **move** the *endpoint* **across** the **screen** 
    - We **do not make it unit** as its **faster**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126044745609.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126050243612.png)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220126050256202.png)

## Adding spheres

- Equation of sphere centered at origin of radius $R$ 

$$
x^2+y^2+x^2=R^2
$$

- For some $(x,y,z)$ if $x^2+y^2+z^2 = R^2$ then $(x,y,z)$ is *on the sphere* and **otherwise not**
  - Uglier when the **sphere center** is at $(cx, cy, cz)$ 
