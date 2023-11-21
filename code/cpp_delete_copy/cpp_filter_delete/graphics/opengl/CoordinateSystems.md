# Coordinate Systems

- last chapter $\to$ learnt how to **use matrices** to advantages via transforming **all matrices** with *transformation matrices*
  - OpenGL expects **all vertices**, that we want to **become visible**
    - To be in **normalized device coordinates** after *each vertex shader* has **run**
- That is $\to$ the **x , y ,z** coordinates of **each vertex** should be between **-1.0** and **1.0**
  - Coordinates out of this range **are not visible**
    - Often $\to$ just **specify** the **coordinates** in a *range / space* that **we determine ourselves** and within the **vertex shader**  
      - We **transform** the *coordinates* to **normalized device coordinates** *NDC*
        - These **NDC** given to the **rasterizer** to *transform* to **2D coordinates / pixels** on *your screen*.

---

- Transforming **coordinates** to **NDC** accomplished in a **step by step** mode
  - Where we **transform** an **objects vertices** in *several coordinate systems*, before transforming them to **NDC**
- The *advantage* of **transforming** to *many intermediate coordinate* systems is that **some operations/calculations** are *easier* in *certain coordinate systems*
  - As is **soon** is *made apparent*
    - There are **5 coordinate systems** that are **important to us**

---

1. Local space / Object Space
2. World Space
3. View space / Eye Space
4. Clip Space
5. Screen space

---

- These are all at **different states** at which our **vertices** will be **transformed** before ending as **fragments**.

---

### Global Picture

- To **transform coordinates** from one space > other
  - Uses **several transformations** *matrices* of **which** the **most important** are *model / view / projection matrix*

- Our **vertex coordinates** start in the **local space** as **local coordinates**
  - They are **further processed** to **world coordinates / view coordinates / clip coordinates**
    - Eventually end as **screen coordinates.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210708083415022.png)

1. **Local coordinates** $\to$ coordinates of the object **relative** to **its local origin**

   - They are the **initial coordinates**

2. Next step $\to$ transform **local coordinates** to **world-space coordinates** which are the cube above placed with other objects in entire world space.

     Relative to **same global origin** , together with many objects also **relative** to this **worlds origin**

3. Then we **transform** the **world coordinates** to **view space**

   - In such a way that **each coordinate** is seen as **from the camera** or **views pointer of view.**

4. After the **coordinates** are in **view space**

   - Want **to project** them to **clip coordinates.** $\to$ these are processed to **-1.0** and **1.0** range and **determine** which vertices actually **end up on screen**
     - projection to **clip space coordinate's** adds **perspective** if using **perspective projection**

5. Lastly transform **clip coordinates** in a process called **==viewport transform==**

   - Transforms the **coordinates** from **-1.0** to **1.0** to coordinate range defined via **glViewPort**

     - Resulting coordinates sent to **rasterizer** to **convert to fragments.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210709013020050.png)

https://gdbooks.gitbooks.io/legacyopengl/content/Chapter4/CoordinateTransforms.html

- Some spaces are **more sensible** in **certain spaces** and easier to use in **certain coordinate systems**
  - Example $\to$ when **modifying** your object $\to$ make most sense to **do in local space**
    - While **calculating** certain operations on **the object** with **respect** to **the position** of the **other objects** make *most sense* *world coordinates*
- Can define **one transformation** matrix that goes from **local to clip space** in **one go**
  - Leaves **less flexibility.**

### Local space

- Coordinate space local the **your object**
  - i.e $\to$ where the **object begins**
- Imagine you created in **cube** modelling software such as **blender**
  - The **origin** is **(0,0,0)** even if the cube may end **up at different location** in the **final application**
    - Most models you have created have this as **initial position**
- All **the vertices** of *your model* are thus in **local space**
  - They are **all local to the object**
    - The vertices of the **container** we have **been using** as coordinates between **-0.5** and **0.5** with **0.0** as its **origin**
      - These are **local coordinates**

### World space

- Importing all objects into one application would leave each one at **0,0,0**, not what we want

  - must define a **position** for **each object** to position **inside a larger world**

    - The **coordinates** of **world space** are *what they sound like*

      - Coordinates of all the **vertices** relative so some **game world**
        - This is the **coordinate space** where you want the objects to be **transformed** in such a way that **they are all scattered** around the place (in some **realistic fashion**)

      - The **coordinates** of the object are **transformed** from **local** to **world space**
        - This is **accomplished** with the **model matrix**

- The **model matrix** is a *transformation* that **translates / scales  and or rotates** the object to the place in the world at a **location orientation** that they **reside within**

  - Like transforming a house by **scaling it down** (bit large in **local space**)
    - Translating to suburbia town and then **rotating to left** on **y axis** so it **fits in the neighbourhood**.

- Think of the **matrix** in the **previous chapter** to **position** the **container** all over the **scene** as a sort of **model matrix** also
  - We **transformed** the *local coordinates* of the **container** to **some different place** in the **scene / world**

### View space

- The **view space** is what the **camera sees** in **OpenGL**, also called **camera space / eye space**
  - The *view space* is made from **transforming** the **world space coordinates** to **coordinates** that are **infront** of the **users view**
    - The **view space** is therefore the space seen from the **cameras point of view**
      - Often accomplished via a **combination of translations** and **rotations** to *translate rotate* the **scene** so certain items are **transformed** to the **front** of the **camera**
        - These **combined** transformations are often stored inside a **view matrix** that *transforms* world coordinates to **view space**
          - next chapter $\to$ discuss how to create a view matrix to simulate a camera.

### Clip space

- At the **end of each vertex shader run**
  - OpenGL expects **the coordinates** to be in some **specific range** and any one **outside the range** is **clipped**
    - Coordinates are **clipped and discarded**, therefore the **remaining coordinates** end as **fragments** that are **visible** on the **screen**
      - This is where the name comes from

- Specify all **visible coordinates** to be in range of **-1 to 1** is **not intuitive** 
  - Specify our own coordinate to **work** in and **convert** back to **NDC** as **OpenGL** expects them
- To **transform vertex coordinates** from **view > clip space**
  - Define a **projection matrix**
    - Specifies a **range such as -1000 to 1000** in each dimension
      - The **projection matrix** then *transforms coordinates* within this **specified** range to **NDC**
        - Any coordinate **out of range** $\to$ is **not mapped between** -1 and 1 therefore **clipped**
- With this **range specified** $\to$ in the **projection matrix**, coordinate of **(1250, 500, 750)** is **not visible**
  - as the **x coordinate** is **out of range** therefore gets converted to higher than 1 in **NDC and clipped**

> Note that if only a part of a primitive e.g. a triangle is outside the clipping volume OpenGL will reconstruct the triangle as one or more triangles to fit inside the clipping range.

- This **viewing box** the **projection matrix** creates is a **frustum**
  - As *each coordinate* that ends inside the frustum end **on user screen**
    - The **total process** to *convert coordinates* within a **specified range** to **NDC** that is **easily mapped** to **2D view space** called **projection** 
      - As the **projection matrix** *projects* **3D** coordinates to the **easy to map 2D NDC**
- Every **vertex transformed** $\to$ to **clip space** a final operation of **==perspective division==** is *performed* 
  - This involves **dividing x y and z** components of **position vector** by the **vectors homogenous w component**
    - perspective division is what **transforms 4D clip to 3D NDC**
      - This is done **automatically** at the *end* of the **vertex shader step**
- After this stage $\to$ the **resulting coordinates** are *mapped* to **screen coordinates**
  - Use the settings **of glViewPort**
    - Then turned into **fragments.**
- The **projection matrix** to *transform* *view coordinates* to **clip coordinates** takes **two different forms**
  - Where **each form** defines its **own unique frustum**
    - Can either **create ** **==orthographic==** projection matrix or **==perspective projection matrix==**

## Orthographic projection

- An **orthographic projection matrix** defines a **cube like frustum** that defines the **clipping space**
  - Any **vertex outside** said box is **clipped**
    - When creating **orthographic projection matrices** $\to$ specify the **width / height / length** of the **visible frustum**
      - All coordinates that are **inside** end up within the **NDC range** after **transformed** by **its matrix** and therefore **wont be clipped**
        - The **frustum** is a **bit like a container**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210708095434271.png)

- Frustum defines the **visible coordinates** 
  - Specified by **width / height / near / far plane**
    - Any coordinate in **front** of the **near plane** clipped and **same** to **far plane**
      - The **orthographic frustum** will directly **map** *every coordinate* inside the frustum to **NDC** with **no side effects** as there is no touch of **w coordinate** of **transformed vector**
        - If w remains 1 **perspective division** will **not change the coordinates**
- To create **orthographic projection** $\to$ use **GLM** `glm::ortho`

```c++
/*
	param 1/2 > specify the left and right coordinate of the frustum
	
	param 3/4 > specify bottom and top part of frustum
	
	param 5/6 define distances between near and far plane
	
	This specific projection matrix transforms all coordiantes between the x, y ,z range values to NDC
*/
glm::ortho(0.0f, 800.0f, 0.0f, 600.0f, 0.1f, 100.0f);
```

- An **orthographic projection matrix** directly *maps* coordinates to **2D** plane that is **your screen**
  - REALITY $\to$ produces **unrealistic results** as the *projection* doesn't **take perspective** into account.
    - This is something the **perspective projection** matrix fixes for us.

## Perspective projection

- Further away is smaller in real life
  - This is **perspective**
    - Noticeable when looking down **infinite motorway** or railway seen below

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210708100304958.png)

- Due to **perspective** $\to$ the **lines** seem to **coincide** as **far enough distance**
  - This is **exactly the effect** that projection attempts to **mimic**
    - Does via using **==perspective projection matrix==** 
- The **projection matrix** maps a **given frustum** range to **clip space**
  - Also **manipulates** the `w` value of **each vertex coordinate** in a way that **further away** a **vertex** from the **viewer**
    - the **higher the w component becomes**
      - Once coordinates are **transformed** to **clip space** $\to$ in range **-w to w** (anything outside clipped)
        - OpenGL needs all **visible coordinates** falls between the -1 to 1 as **final vertex shader output**
          - therefore **once the coordinates** are *in clip space* $\to$ *perspective division* is **applied to clip space coordinates.**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210708100651060.png)

- Every **component** of the **vertex coordinate** divided by the **w coordinate** giving **smaller vertex coordinates**
  - For when it **gets further away** a **vertex from the viewer**
- This is alternative reason to why **w is important**
  - Helps with **perspective projection**
    - The **resulting coordinates** are **then in ==normalized device space==** 
    - http://www.songho.ca/opengl/gl_projectionmatrix.html - maths behind the projections

---

- The **perspective projection matrix** can be created in **GLM as follows**

```c++
/*
	param 1 > defines the fov value, defines how large the viewspace is
		- realistic view is often 45 degreees, doom style is often higher value
		
	param 2 > sets the aspect ratio calculated by dividing vieport width / height
    
    param 3/4 set near/far plane of frustum, often set near distance to 0.1 and the far distance to 100.0
    
    all the vertices between near/far and in the frustum are rendered.

*/
glm::mat4 proj = glm::perspective(glm::radians(45.0f), (float)width/(float)height, 0.1f, 100.0f);
```

- What **glm::perspective** does is create a **large frustum** defines the **visible space**
  - Anything **outside this frustum** will **not end** up in **clip space volume** and thus clipped
    - A **perspective frustum** visualized as **uniformly shaped box** where *each coordinate* in the box will **be mapped** to a **point** within **clip space**
      - image of **perspective frustum** is *seen below.*

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210708101801456.png)

> - Whenever the *near* value of your perspective matrix is set too high (like `10.0`), OpenGL will clip all coordinates close to the camera (between `0.0` and `10.0`), which can give a visual result you maybe have seen before in videogames where you could see through certain objects when moving uncomfortably close to them.

- When using **orthographic projection**
  - Each of the **vertex coordinates** are *directly mapped* to the **clip space**
    - With **any perspective division** (still does perspective division, but the `w` component is **not manipulated beyond 1**)
      - Therefore has **no effect**.
- Orthographic projection **doesnt** use **p.d** , objects that are **further away** do **not seem smaller**
  - Produces an **odd visual output**
    - For this reason $\to$ the **orthographic projection** used for **2D renders** and some **architectecutral / engineering applications** where we *dont want* the **vertices** being **distorted** by **perspective** 
      - Applications such as **blender** used for **3D models** use **orthographic projection** for *modelling* as it accurately shows **the dimensions**
        - Comparison of both in blender.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210708214228496.png)

- With **perspective projection**
  - The *vertices* that are **farther** appear **much smaller**
    - While in **orthographic** $\to$ each vertex has **same distance to the user.**

## Putting all together.

- Create a **transformation matrix** for *each* of the **aforementioned steps**
  - Model, view and **projection matrix**
    - A **vertex** coordinate is **then transformed** to **clip coordinates** as *follows*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210708220519331.png)

- THe **order** of the **matrix multiplication**
  - Is **reversed** (*remember* that we **need** to **read matrix multiplication**) from *right to left*
    - The **resulting vertex** should be **assigned** to **gl_Position** in *vertex shader* then **OpenGL** automatically **perform** *perspective division* and **clipping**

> **And then?**
> The **output of the vertex shader** requires the **coordinates** to be in **clip-space** which is what **we just did with the transformation matrices.** OpenGL then performs ***perspective division*** on the ***clip-space coordinates*** to **transform them to *normalized-device coordinates***. OpenGL then uses the **parameters** from **glViewPort** to **map the normalized-device coordinates** to ***screen coordinates*** where **each coordinate** corresponds to a **point on your screen** (in our case a 800x600 screen). This process is called the ***viewport transform***.

---

## 3D

- Know how **to transform** 3D to 2D
  - Can start **rendering** real **3D objects** instead of 2D.
- First create a **model matrix**
  - This consists of **translations scaling and/or rotations** 
    - that we wish to **apply** to *transform* each of the **objects vertices** to the **global world space.**

- Transform the plane a bit via **rotation** on the **x axis**
  - appears as if its **laying on the floor**
    - model matrix looks like this.

```c++
glm::mat4 model = glm::mat4(1.0f);
model = glm::rotate(model, glm::radians(-55.0f), glm::vec3(1.0f, 0.0f, 0.0f)); 
```

- Via multiplying the **vertex coordinates** with this **model matrix**
  - We **transform** the **vertex coordinates** to **world coordinates**
    - Our plane is **slightly** on the *floor* therefore **represents** the **plane** in the **global world**
- Next $\to$ create a **view matrix**
  - Want to **move slightly** backwards in the **scene** so the **object** becomes **visible**
    - When in **world space** $\to$ located at **origin** (0,0,0)
      - To **move around the scene** $\to$ consider following:
        - To move a **camera backwards** $\to$ same as **moving scene forward**
- This what the **view matrix** does
  - Move the **entire scene** around *inversed* where we **want** the **camera to move**
    - As we wish to **move backward** $\to$ As openGL is **right handed**, move in the **positive z axis**
      - Translate the scene **towards negative z axis**
        - Gives **impression of moving backwards**

---

### Right handed system

- Conventionally $\to$ OpenGL is a **right handed system**
  - This means the **positive x axis** is to the **right** and positive **y** is **up** and **positive z** is backwards
    - Think of the screen as the centre of **3 axes** and the **positive z axis** going through the **screen towards** you
      - axes drawn as follow.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210708225036233.png)

- Discuss how to **move** around the **scene** in **more detail** next chapter
  - The **view matrix** now looks like this:

```c++
glm::mat4 view = glm::mat4(1.0f);
// note that we're translating the scene in the reverse direction of where we want to move
view = glm::translate(view, glm::vec3(0.0f, 0.0f, -3.0f)); 
```

- Last thing we require to **define projection matrix**
  - Use the **perspective projection** for our scene $\to$ declare the **projection matrix** like so

```c++
glm::mat4 projection;
projection = glm::perspective(glm::radians(45.0f), 800.0f / 600.0f, 0.1f, 100.0f);
```

-  Now that we **created** the **transformation matrices**
  - Should pass to **our shaders**
    - Declare the **transformation** matrices as **uniforms** in the **vertex shader** and **multiply** them with **vertex coordinates.**

```c++
#version 330 core
layout (location = 0) in vec3 aPos;
...
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

void main()
{
    // note that we read the multiplication from right to left
    gl_Position = projection * view * model * vec4(aPos, 1.0);
    ...
}
```

- Send the **matrices** to the **shader**
  - Often just done **each frame** since the *transformation matrices* often **change a lot** 

```c++
int modelLoc = glGetUniformLocation(ourShader.ID, "model");
glUniformMatrix4fv(modelLoc, 1, GL_FALSE, glm::value_ptr(model));
... // same for View Matrix and Projection Matrix
```

- Our vertex coordinates are **transformed via the model, view, projection matrix**

  - Final object should be:
    1. Tilted **backwards** to the **floor**
    2. Slightly **further away**
    3. Displayed with **perspective** (should get **smaller, the further the vertices are**)

  

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210709004033052.png)

- https://stackoverflow.com/questions/8115352/glmperspective-explanation

---

## More 3D

- Working in **2D plane** within **3D SPACE**
  - Extend the 2D plane to a 3D cube
    - To render a **cube** requires **36 vertices** (*6 faces* x 2 triangles x 3 vertices)
      - 36 VERTICES are a lot to sum therefore retrieve from

```c++
model = glm::rotate(model, (float)glfwGetTime() * glm::radians(50.0f), glm::vec3(0.5f, 1.0f, 0.0f));
```

- draw the cube using **glDrawArrays**
  - This time with a count of **36 vertices**

```c++
glDrawArrays(GL_TRIANGLES, 0, 36);
```

- Some sides of the cube are **drawn over the others**
  - This occurs when **OpenGL** draw it **triangle by triangle** and **fragment by fragment**
    - It will just **overwrite any pixel** color that **may have been drawn there**
      - As there are **there is no guarantee** on the **order of triangles** rendered in the **same draw call**
        - Some triangles are **drawn on top** of *one another* even though it should be **front**

- OpenGL stores **depth information** in a **z buffer** that allows OpenGL to decide **what to draw** over a pixel and **when not to**
  - Using the **z buffer**  > configure OpenGL to perform **depth testing**

## Z-buffer

- The **depth buffer** $\to$ auto created by **GLFW**
  - Just like a **color buffer** that stores the **colors** of the **output image**
    - Depth stored in **each fragment** as its **z value**
      - When the **fragments** wants to **outputs it color** $\to$ OpenGL compares **depth value** with **z buffer**
        - if **current fragment** is *behind* the **other fragment** becomes **discarded**, otherwise overwritten
          - Called **==depth testing==** , done **automatically**.

- Enable it with **glEnable** and disable with **glDisable**

```c++
glEnable(GL_DEPTH_TEST);  
```

- Must **clear** it on **each render iteration**. otherwise the **depth information** of the *previous frame* stays in the buffer
  - just like **clearing color buffer** $\to$ can clear **depth**

```c++
glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
```

- Display **multiple cubes** in **different locations**

```c++
glm::vec3 cubePositions[] = {
    glm::vec3( 0.0f,  0.0f,  0.0f), 
    glm::vec3( 2.0f,  5.0f, -15.0f), 
    glm::vec3(-1.5f, -2.2f, -2.5f),  
    glm::vec3(-3.8f, -2.0f, -12.3f),  
    glm::vec3( 2.4f, -0.4f, -3.5f),  
    glm::vec3(-1.7f,  3.0f, -7.5f),  
    glm::vec3( 1.3f, -2.0f, -2.5f),  
    glm::vec3( 1.5f,  2.0f, -2.5f), 
    glm::vec3( 1.5f,  0.2f, -1.5f), 
    glm::vec3(-1.3f,  1.0f, -1.5f)  
};
```

in **glDrawArrays** 10 times being called

- Send a **different model matrix** to the **vertex shader** each time before **sending a draw call**
  - Create a **small loop** within the **render loop** that renders the object 10 times.
    - Add unique rotation on each container.

```c++

glBindVertexArray(VAO);
for(unsigned int i = 0; i < 10; i++)
{
    glm::mat4 model = glm::mat4(1.0f);
    model = glm::translate(model, cubePositions[i]);
    float angle = 20.0f * i; 
    model = glm::rotate(model, glm::radians(angle), glm::vec3(1.0f, 0.3f, 0.5f));
    ourShader.setMat4("model", model);

    glDrawArrays(GL_TRIANGLES, 0, 36);
}
```

https://solarianprogrammer.com/2013/05/22/opengl-101-matrices-projection-view-model/ - understanding projection - view - model better

https://ogldev.org/www/tutorial12/tutorial12.html

http://www.songho.ca/opengl/gl_transform.html

https://www.youtube.com/watch?v=LhQ85bPCAJ8&

https://www.reddit.com/r/opengl/comments/btbgrb/clipping_coordinates_vs_normalized_device/

https://stackoverflow.com/questions/724219/how-to-convert-a-3d-point-into-2d-perspective-projection

https://www.tomdalling.com/blog/modern-opengl/explaining-homogenous-coordinates-and-projective-geometry/

https://stackoverflow.com/questions/25584667/why-do-i-divide-z-by-w-in-a-perspective-projection-in-opengl

https://titanwolf.org/Network/Articles/Article?AID=591ec98a-306f-4411-9425-797441054c56#gsc.tab=0

https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix/projection-matrix-GPU-rendering-pipeline-clipping

https://www.mathematik.uni-marburg.de/~thormae/lectures/graphics1/graphics_6_1_eng_web.html#1

https://math.stackexchange.com/questions/164700/how-to-transform-a-set-of-3d-vectors-into-a-2d-plane-from-a-view-point-of-anoth

http://www.opengl-tutorial.org/beginners-tutorials/tutorial-3-matrices/

https://www.khronos.org/opengl/wiki/Vertex_Transformation

https://stackoverflow.com/questions/2422750/in-opengl-vertex-shaders-what-is-w-and-why-do-i-divide-by-it

https://computergraphics.stackexchange.com/questions/5398/is-placing-z-value-of-vertex-in-w-enough-to-achieve-perspective-projection-in-op



[![Left-handed and right-handed coordinate systems. Source: http://viz.aset.psu.edu/gho/sem_notes/3d_fundamentals/html/3d_coordinates.html](http://www.learnopengles.com/wordpress/wp-content/uploads/2012/10/left_right_hand-300x225.gif)](http://www.learnopengles.com/understanding-opengls-matrices/left_right_hand/)Left-handed and right-handed coordinate systems. Source: http://viz.aset.psu.edu/gho/sem_notes/3d_fundamentals/html/3d_coordinates.html

### The perspective divide

The perspective projection doesn’t actually create the 3D effect; for that, we need to do something called the *perspective divide*. Each coordinate in OpenGL actually has four components, X, Y, Z, and W. The projection matrix sets things up so that after multiplying with the projection matrix, each coordinate’s W will increase the further away the object is. OpenGL will then [divide by w](http://en.wikipedia.org/wiki/Transformation_matrix#Perspective_projection): X, Y, Z will be divided by W. The further away something is, the more it will be pulled towards the center of the screen.

http://www.terathon.com/gdc07_lengyel.pdf

For more intuition of what “divide by W” does…

Look at the projection matrix produced by glFrustum. (see the GL spec). Note that the last row is (0, 0, -1, 0). The result of applying this matrix to an (x,y,z,1) coordinate is that the W component becomes -z, i.e. (x’, y’, z’, -z).

So, when you “divide by W”, you’re really dividing by -z. Don’t worry about the negation. The greater the magnitude of z, the further the coordinate is from the viewing position. Thus, dividing x’ and y’ by z makes further coordinates move closer to (0, 0). So distant objects appear smaller > WHICH IS TO THE CENTRE > THIS IS WHAT IT MEANS.

https://www.scratchapixel.com/lessons/3d-basic-rendering/perspective-and-orthographic-projection-matrix/opengl-perspective-projection-matrix - DERIVATION OF PROJECTION MATRIX

---

### Projection Matrix derivation

- vertex data > eye space / camera space > clip space > NDC (division by w component) > viewport transform.
- frustum culling and NDC are **combined** in the **projection matrix** of *open GL*
- Building the matrix from the: *left , right, bottom, top, near and far* **boundary values**

---

- Frustum culling $\to$ occurs in the **clip coordinates**

https://www.youtube.com/watch?v=YiFwvAFk-xs - similar triangles.

https://ogldev.org/www/tutorial12/tutorial12.html - just watch this makes sen

https://m.youtube.com/watch?v=Q2uItHa7GFQ - homogenous coordinates.

---

![The Viewing Frustum](https://lh3.googleusercontent.com/proxy/D2ISz7SiFlOP3W_XsBFL3nD33qHZ0TOCSTqUiHWncYINYGHEoo-khkiaQN1_1JJYv3C9npyPryCoBSkFI_8YgoFgyC7n26bAWT_9mF8)

http://deltaorange.com/2012/03/08/the-truth-behind-homogenous-coordinates/
