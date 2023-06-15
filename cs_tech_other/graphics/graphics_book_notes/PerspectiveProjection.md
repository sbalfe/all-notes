Perspective Projection

<img src="https://cdn.discordapp.com/attachments/309758102582984705/911405540100431952/image0.jpg" alt="img" style="zoom:67%;" />

- basic projection matrix just scales the near and far by near plane, default is 1 but this will be replaced by a FOV to control the zoom scale.

# Full Derivation

1. Begin with the concept of the **z divide** , points further away are **greater** in the **z value** they have therefore must appear **further away** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119170347917.png)

   - Thus we obtain the **desired** equations of $\large x' = \frac{xd}{z}$ and $\large y'=\frac{yd}{z}$ 
     - Conventionally $d=1$ as the **distance** from the **camera**
     - These formulaes may be **calculated** from the **similar triangles concept also**.
   - This hints that we require a **method** to **copy** $z$ into the **w** part of **the matrix** as this deals with **homogenous vectors**
   
2. Building the **basic perspective matrix** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119194708245.png)

   - So we apply **scaling** first to $x$ and $y$ by the **near plane** which will be **controlled later** to shift the camera about , which is determined by **resolution** and **angle of FOV** 

   - For the $z$ values, this explanation is sufficient , and uses the mapping of $z$ to $z^2$ as to **retain depth information** after **perspective divide** 

     <img src="https://cdn.discordapp.com/attachments/309758102582984705/911405540100431952/image0.jpg" alt="img" style="zoom: 67%;" />

   - Finally just set the **final value** of **z column** to $1$ to **copy** it over to the final matrix $w$ value which thus **becomes** $z$ as shown above.

3. From this matrix we simply just **multiply** it by the **orthographic one** to obtain the **baseline perspective projection matrix**.

   - So our orthographic view volume and perspective matrix are multiplied together

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119012330805.png)

     

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211119194708245.png)
     
     - This gives us the following, this works because the **perspective matrix** $P$ translates the **frustum** into an **orthographic volume** bounded with **standard planes**, thus just **apply orthographic translation** to **canonical volume**.
     
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120005524178.png)
     
       - Similar to **openGl** implementation:
     
![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20211120005554138.png)

![What does a perspective projection matrix look like in OpenGL? - Game  Development Stack Exchange](https://i.stack.imgur.com/C9PST.jpg)

- This is opengls one , the **axis** is **switched about**

# Further research

https://www.youtube.com/watch?v=_EhY31MSbNM

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20220113163151107.png)
