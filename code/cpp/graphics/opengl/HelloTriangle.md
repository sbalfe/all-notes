# Hello Triangle

- OpenGL > everything is in **3D space**
  - The screen / window is a **2D array of pixels** so lots of work is done about **transforming** 3D to 2D so **they fit**.
- Process of **3D TO 2D transformations** is managed by the **graphics pipeline**
  - Divided into **two large parts**
    1. Transform **3D coordinates** > **2D coordinates** 
    2. Transforms **2D coordinate** into **colored pixels** 

---

- The **graphics pipeline** take as **input** > a set of **3D coordinates** and *transforms* to **colored 2D pixels** on the **screen**.
  - The **GP** divided into **divided into several steps** where *each step requires* the **output** of the **previous step** as **its input**.
- All of **these steps** are **highly specialized** $\to$ (they have one specific function) And Can **easily be executed in parallel**.
  - Because of the **parallel nature** $\to$ graphics cards have **thousands of small processing cores** to *quickly* process your **data** within the **graphics pipeline**

- Processing **cores** run **small programs** on the **GPU** for **each step** of the **pipeline**
  - These are **small programs** called **shaders**
    - Some are **configurable** by the **developer** $\to$ allows **us to write our own shaders** to *replace existing default shaders*.
      - More **fine grained control** over **specific parts** of the **pipeline** and **because** they *run on the GPU* 
        - Save us **valuable CPU time**.
- Shaders **written** in OpenGL Shading Language.
  - We delve **more into in the next chapter**.
    - Find **abstraction representation** of *all the stages* of the **graphics pipeline**
      - The **blue sections represent** sections where we **inject our own shaders**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210616181715259.png)

- The **graphics pipeline** contains **larger number of sections**
  - Each handle **one specific part** of *converting* the **vertex data** to a **fully rendered pixel**
    - We **briefly** explain **each part** in a **simple way** to give you **good overview of how the pipeline operates**.

---

- As **input** to the **graphics** pipeline $\to$ **pass** in a list **of three 3D coordinates** that form a **triangle** in **an array**
  - Called ==**Vertex Data**== $\to$ this **vertex data** is a **collection of vertices**
    - A ==vertex== is a **collection** of **vertices**
    - A **vertex** is a *collection* of data **per 3D coordinates**
      - This **vertex's data** is *represented* using ==**vertex attributes**== that *contain any data we wish*
      - Simply assume each **vertex** of just a **3D position** and **some color value**.

>- In order for **OpenGL** to know what to **make of your collection of coordinates** and **color values OpenGL** requires you to hint what **kind ** of **render types** you **want to form with the data**. 
>- Do we want the **data rendered** as a **collection of points**, a **collection of triangles** or perhaps **just one long line**? Those hints are called **primitives** and are **given to OpenGL** while **calling any of the drawing commands**. 
>- Some of these hints are **GL_POINTS**, **GL_TRIANGLES and GL_LINE_STRIP**.

---

- **FIRST PART** of pipeline $\to$ the ==**vertex shader **==that takes as **input** a **single vertex**
  - Main **purpose** of the *vertex shader* $\to$ transform **3D coordinates** into **different 3D coordinates** (*later*).
    - Allows us to do **basic processing** on the **vertex attributes**.

---

- The next part is ==**primitive assembly**==
  - Takes as **input** all *the vertices* (or vertex if **GL_POINTS**) is chosen from the **vertex shader**
    - Form a **primitive** and **assembles all the points** in the *primitive shape given*. 
      - For this case $\to$ a **triangle**. 

---

- The **output** of the **primitive assembly stage** is passed to ==**geometry shader**==
  - This **geometry shader** takes as **input** a *collection of vertices* that **form a primitive** 
    - Can generate **other shapes** via **emission** of **new vertices** to form **new / other primitives**
    - Example case $\to$ generates a **second triangle** out **of given shape**

---

- The **output** of **geometry shader** passed to **==rasterization stage==** 
  - This will **map the resulting primitive** to the *corresponding pixels* on the **final screen**
    - Results in **fragments** for the **fragment shader** to use 
      - Before the **fragment shaders** can *run* $\to$ ==clipping performed== (*discards **fragments that are outside the view***) thus *increasing performance.*

> Fragment in **OPENGL** is all the **data** required for it to **render a single pixel**

- The **purpose** of the ==**fragment shade**r== is to **calculate** the *color* of a **pixel**
  - This is the **stage** where all **advanced OGL** effects occur.
    - Contains data on the **3D scene** used to **calculate the final pixel color**
      1. Lights
      2. Shadows
      3. Color
      4. Light etc.

---

- After all the **corresponding colour values** are *determined*.
  - Final object pass through **final stage** called ==**alpha test / blending stage**== 
    - This checks the ==**corresponding depth**== (alongside *stencil value*) of the fragment $\to$ uses this to **check the resulting fragment** is *front / behind other objects* and **discarded accordingly**. 
    - Checks for ==alpha vales== (define **opacity**) , **blends** the *object accordingly*
      - Even if a **pixel output color** is *calculated* **in the fragment shader** $\to$ the **final pixel color** could be **entirely different** on the *rendering* of **multiple triangles**.

---

- Often only working with **vertex / fragment shader**
  - The **geometry shader** $\to$ is *optional* and often left to **default shader**
- There is a **==tessellation stage==** for *transform feedback loop* 

---

- For **modern OpenGL** $\to$ required to **define a vertex / fragment shader** of our own (*no defaults*)

---

## Vertex Input

- To begin **drawing** $\to$ first give **OpenGL** some **input vertex data**
  - OpenGL is **3D GL** therefore all *coordinates* we specify are **3D** (x, y, z coordinates).
  - Does **not transform** all the **3D coordinates** to **2D pixels** on the screen
    - it only processes **3D coordinates** in the **range of** $-1.0$ / $1.0$ on **all 3 axes** (x, y, z)
- Every coordinate within **so called normalized device coordinates** range ends up **visible on the screen**, coordinates **outside wont**.

---

```c++

float vertices[] = {
    -0.5f, -0.5f, 0.0f,
     0.5f, -0.5f, 0.0f,
     0.0f,  0.5f, 0.0f
};  
```

- OpenGL works in **3D space** $\to$ render **2D triangle**
  - With **each vertex** having a **z coordinate** 

> **Normalized Device Coordinates (NDC)**
>
> - Once your **vertex coordinates** have been **processed in the vertex shader**, they should be in **normalized device coordinates** which is a small space where the `x`, `y` and `z` values vary from `-1.0` to `1.0`. 
> - Any coordinates that **fall outside this range will be discarded/clipped** and won't be visible on your screen. Below you can see the triangle we specified **within normalized device coordinates** (ignoring the `z` axis):
>
> ![2D Normalized Device Coordinates as shown in a graph](https://learnopengl.com/img/getting-started/ndc.png)
>
> - Unlike **usual screen coordinates** the **positive y-axis points** in the **up-direction** and the `(0,0)` coordinates are at the **center of the graph,** instead of top-left. 
> - Eventually **you want all the (transformed) coordinates** to end up in **this coordinate space,** otherwise they won't be visible.
>
> - Your **NDC coordinates** will then be **transformed to screen-space coordinates** via the **viewport transform** using the **data you provided** with **glViewport**. 
> - The **resulting screen-space coordinates** are then **transformed to fragments** as inputs to your **fragment shader**.

- The **vertex data** is *defined*
  - Send as **input** to **first process** of the **GP** ==vertex shader==. 
    - Done via creation of **memory** on the **GPU** where you **store vertex data**
      - Configure how **OpenGL** should *interpret memory* and *specify* **how to send data** to the **graphics card**
  - Vertex shader $\to$ *processes* as **many vertices** as we **say** it to **from memory**

---

- Memory of **this** managed by **vertex buffer objects** (*VBO's*) 
  - Stores **larger numbers of vertices** in **GPU memory**.
    - *Advantage* of **those buffer objects** is we can **send large batches of data** at *once* to the **graphics card**.
    - We keep if there's **enough memory left**.
      - Without sending **data one vertex at a time**
- Sending **graphics** from **CPU** is *relatively slow*
  - Wherever we **can** $\to$ *try* to send **as much as possible at once**
    - Once the **data** is in the **graphics card** memory $\to$ the *vertex shader* has *near instant access* to the **vertices** $\to$ *extremely fast*.
- A **vertex buffer** $\to$ is the **first occurrence** of an **opengl object**

```
unsigned int VBO;
glGenBuffers(1, &VBO);  
```

- OpenGL has many types of **buffer objects** and the buffer type of a **vertex buffer object is GL_ARRAY_BUFFER**. OpenGL allows us to bind to several buffers at once as long as they have a different buffer type. We can bind the newly created buffer to the GL_ARRAY_BUFFER target with the glBindBuffer function:

```
glBindBuffer(GL_ARRAY_BUFFER, VBO);  
```

- From that point on any **buffer calls we make (on the GL_ARRAY_BUFFER target)** will be used to **configure the currently bound buffer**, which is VBO. 
- Then we can make a **call to the glBufferData** function that copies the previously defined vertex data into the buffer's memory:

```
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
```

- glBufferData is a function specifically targeted to copy user-defined data into the currently bound buffer. Its first argument is the type of the buffer we want to copy data into: the vertex buffer object currently bound to the GL_ARRAY_BUFFER target.
-  The second argument specifies the size of the data (in bytes) we want to pass to the buffer; a simple `sizeof` of the vertex data suffices. The third parameter is the actual data we want to send.

- Fourth parameter $\to$ how the GPU should manage said data
  - 3 **forms**
    1. GL_STREAM_DRAW - data set **only once** and used by the **GPU** at *most a few times*
    2. GL_STATIC_DRAW - data set **only once** and used **many times**
    3. GL_DYNAMIC_DRAW - data change **a lot** and used **many times**

- **position data** of the *triangle* does *not change*
  - Its used **a lot** and **stays same** for **every render** so that's why **static draw** is ideal.
- If some **buffer** with *data* that is **likely to change often** $\to$ use GL_DYNAMIC_DRAW 
  - Ensures the **GPU** place data in memory that **allows faster writes**

---

- For now $\to$ we **stored** the **vertex data** within **memory** on the **graphics card**
  - Managed by a **vertex buffer object** called `VBO` . 
- Next $\to$ create **vertex fragment shaders** to **process the data**

## Vertex Shader

- One of the **programmable** sections
  - Modern OGL requires to set up a **vertex / fragment shader** if we **want to do some rendering**
    - Briefly introduce shaders and configure **two simple ones** for **first triangle**. 

- First thing is to write a **vertex shader** 
  - Using **OpenGL shading language** 
    - Then **compile shader** so we can use **in our application**

```c++
#version 330 core
layout (location = 0) in vec3 aPos;

void main()
{
    gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);
}
```

- GLSL is **similar to C**
  - Each shader > has a **version declaration** 
  - As OpenGL 3.3 and higher the **version numbers** of GLSL match the version of **OpenGL**
  - Explicit mention of **core profile usage**.

---

- Then we **declare** all the **input vertex attributes** in the **vertex shader** using the ==**in**== keyword
  - Focus on **position data** $\to$ require a **single vertex attribute** 
- GLSL $\to$ has **vector datatype** that contains **1 to 4 floats** based on the *postfix digit*.
  - Each vertex has a **3D coordinate** $\to$ therefore create ==**vec3**== *input variable* with the name of **aPos** 
- Set the **location** of said input variable via `layout (location = 0)` 
  - See later why **you need that location**.

> **VECTOR**
>
> - In **graphics programming** $\to$ use **vector** often as it **neatly represents** positions and **directions** in *any space* with **useful mathematical properties**
>
> - A *vector* in **GLSL** $\to$ has **max size** of **4**
>   - Each value **retrieved** via *vec.x* / *vec.y* / *vec.z* / *vec.w* respectively. 
>     - Each of these represent **coordinate in space**
>     - ==vec.w== $\to$ used for ==**perspective division**==.

- To set the **output** of **some** *vertex shader*
  - Must assign the **position data** to the **predefined** `gl_position` variable which is **vec4** behind the scenes
- End of the **main function** $\to$ whatever **gl_Position** is is the **output** of the **vertex shader**.
  - Our input is a vector of **size 3** therefore cast to **vector** of **size 4**.
    - Do this via inserting **vec3 values** into the **constructor** of **vec4** and set **w component** to **1.0f** (explained below)
- Current **vertex shader** is very **simple vertex shader**
  - we did **no processing** on the input data and **forwarded** it to the **shaders output**
  - Real apps $\to$ the **input data** is often **not already** in a **normalized device coordinates** therefore first transform the **input data** to **coordinates** that are within the **visible OGL region**.

## Fragment shader

- Fragment shader $\to$ **second / final shader** for this triangle.
  - Fragment shader concerned with **colour** output of **the pixels**
    - Keep things simple $\to$ always output **same orangeish colour**.

>- Colours in computer graphics are represented as an array of **4 values**: the **red, green, blue and alpha (opacity)** component, commonly abbreviated to RGBA. 
>- When defining a color in **OpenGL or GLSL we set the strength of each component to a value between `0.0` and `1.0`.**
>-  If, for example, we would set red to `1.0` and green to `1.0` we would get a mixture of both colors and get the color yellow. Given those 3 color components we can generate over **16 million different colors!**

```c++
#version 330 core
out vec4 FragColor;

void main()
{
    FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f);
} 
```

- Only requires **one output variable** of size **vec4** to define the **final colour output** that we **calculate ourselves**
  - Declare **output values** with ==**out keyword**== that was **named** `FragColor` 
    - Assign a `vec4` to the **color output** as an **orange color** with **alpha value** of 1.0 (**fully opaque**).

- Process of **fragment shader** compilation similar to vertex
  - Just use **GL_FRAGMENT_SHADER** now.

```c++

unsigned int fragmentShader;
fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
glShaderSource(fragmentShader, 1, &fragmentShaderSource, NULL);
glCompileShader(fragmentShader);
```

### Shader program

- A shader program object is the final linked version of multiple shaders combined. To use the recently compiled shaders we have to link them to a shader program object and then activate this shader program when rendering objects. The activated shader program's shaders will be used when we issue render calls.

- When linking the shaders into a program it links the outputs of each shader to the inputs of the next shader. This is also where you'll get linking errors if your outputs and inputs do not match.

Creating a program object is easy:

```
unsigned int shaderProgram;
shaderProgram = glCreateProgram();
```

- The glCreateProgram function creates a program and returns the ID reference to the newly created program object. Now we need to attach the previously compiled shaders to the program object and then link them with glLinkProgram:

```
glAttachShader(shaderProgram, vertexShader);
glAttachShader(shaderProgram, fragmentShader);
glLinkProgram(shaderProgram);
```

- The code should be pretty self-explanatory, we attach the shaders to the program and link them via glLinkProgram.

Just like shader compilation we can also check if linking a shader program failed and retrieve the corresponding log. However, instead of using 

glGetShaderiv

 and 

glGetShaderInfoLog

 we now use:

```
glGetProgramiv(shaderProgram, GL_LINK_STATUS, &success);
if(!success) {
    glGetProgramInfoLog(shaderProgram, 512, NULL, infoLog);
    ...
}
```

- The result is a program object that we can activate by calling glUseProgram with the newly created program object as its argument:

```
glUseProgram(shaderProgram);
```

- Every shader and rendering call after glUseProgram will now use this program object (and thus the shaders).

- Oh yeah, and don't forget to delete the shader objects once we've linked them into the program object; we no longer need them anymore:

```
glDeleteShader(vertexShader);
glDeleteShader(fragmentShader);  
```

- Right now we sent the input vertex data to the GPU and instructed the GPU how it should process the vertex data within a vertex and fragment shader. We're almost there, but not quite yet. 
- OpenGL does not yet know how it should interpret the vertex data in memory and how it should connect the vertex data to the vertex shader's attributes. We'll be nice and tell OpenGL how to do that.

## Linking Vertex Attributes

- Vertex shader **allows** us to **specify** *any input* we want in form of a **vertex attribute**
  - This allows **great flexibility** $\to$ means we must **specify manually** what part of **our input data** goes to **which vertex attribute** in the **vertex shader**.
    - We must *specify* how **OpenGL** should *interpret* the **vertex data** *before rendering*

---

- **Vertex buffer data** formatted as such

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210616225542317.png)

- POS data stored as **32 bit 4 byte floating point values**
- Each one has **3 values**
- There is **no space** or *other values* between each set of 3 values
- They are **tightly packed** in the array
- The **first value** in the data is the **start of the buffer**.

---

- Using this information tell openGl how it **should interpret** the *vertex data* (**per vertex attribute**) via glVertexAtrribPointer

```c++
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);  
```

- Each **vertex attribute** $\to$ takes data from **memory managed** by a **VBO**
  - Which **VBO** it takes data (multiple possible )from **determined by** the **VBO** bound to **GL_ARRAY_BUFFER** when calling **glVertexAttribPointer**
  - As the **previously** defined **VBO** is **still bound** before **calling glVertexAttribPointer** , vertex attribute $0$ now **associated** with **its vertex data**.

---

- We have **specified** how **OGL** should **interpret** the **vertex data**
  - We also **enable the vertex attribute**
- Everything is **now setup**
  1. Init the **vertex data** in a **buffer** using a **VBO**
  2. set up **vertex / fragment shader** and told **OGL** how to **link the vertex data** to the **vertex shaders vertex attributes**

```c++

// 0. copy our vertices array in a buffer for OpenGL to use
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
// 1. then set the vertex attributes pointers
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);  
// 2. use our shader program when we want to render an object
glUseProgram(shaderProgram);
// 3. now draw the object 
someOpenGLFunctionThatDrawsOurTriangle();   
```

- Must be repeated **every time** to draw
  - May not look much, but if there are **over 5 vertex attributes** and possibly **100s** of **objects**
    - **Binding the correct buffer** and *configuring* all **vertex attributes** for each objects is often **cumbersome**
- Possibly a way to **store** all these **state configurations** into an **object** and just **bind this object** to **restore state**.

## Vertex Array Object

- A **vertex array object** **==VAO==** is able to be **bound** like a **vertex buffer object** and any *subsequent* vertex **attribute** calls from that point are **stored** within this **VAO**.
  - This **advantage** that when **configuring** the **vertex attribute pointers**
    - Only have to **make those calls** once and **whenever we want to draw** $\to$ just **bind the correct VAO**

- Makes **switching** between different **vertex data and attribute configurations** *easy* as **binding a different VAO**
  - All the **state** we just set is **stored in the VAO**

> Core open GL requires that we use a **VAO** so it knows what to do with **our vertex inputs**
>
> failing to bind a VAO will most likely **prevent drawing anything**

---

- **Vertex array** object stores the following:
  1. Call to `glEnableVertexAttribArray` or `glDisableVertexAttribArray`
  2. Vertex **attribute** *configurations* via `glVertexAttribPointer`
  3. Vertex **buffer objects** *associated* with **vertex attributes** by *calls* to `glVertexAttribPointer`

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210616233337018.png)

- Process to generate a **VAO** similar to **VBO**

```c++
unsigned int VAO;
glGenVertexArrays(1, &VAO);  
```

- To use a **VAO** $\to$ just bind the **VAO** using `glBindVertexArray` 
  - From **that point onwards** $\to$ should **bind / configure** the **corresponding VBO's** and **attribute pointers** and then **unbind VAO** for *later use*.
    - As soon as you **wish to draw** $\to$ *bind the VAO* with the **preferred settings** *before* **drawing the object** 

```C++
// ..:: Initialization code (done once (unless your object frequently changes)) :: ..
// 1. bind Vertex Array Object
glBindVertexArray(VAO);
// 2. copy our vertices array in a buffer for OpenGL to use
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
// 3. then set our vertex attributes pointers
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);  

  
[...]

// ..:: Drawing code (in render loop) :: ..
// 4. draw the object
glUseProgram(shaderProgram);
glBindVertexArray(VAO);
someOpenGLFunctionThatDrawsOurTriangle(); 
```

## Element Buffer Objects.

- Suppose you want to draw a **rectangle** instead of a **triangle**
  - draw using **two triangles.** (OpenGL works with **triangles**)
    - generate the **following set of vertices**.

```c++
float vertices [] = {
    // first triangle
    0.5f, 0.5f, 0.0f, // top right
    0.5f, -0.5f, 0.0f, // bottom right
    -0.5f, 0.5f, 0.0f, // top left
    
    // second triangle
    0.5f, -0.5f, 0.0f, // bottom right
    -0.5f, -0.5f,0.0f, // bottom left
    -0.5f, 0.5f, 0.0f // top left

}
```

- There is **clear overlap**.
  - specify **bottom, right** and **top left twice**.
    - Overhead of **50% since** *the same rectangle* can be specified with only **4 vertices**.

- This rapidly gets **worse** as we have get **more than 1000 triangles**
  - there is massive overlap.
- Better solution $\to$ store **unique vertices** then *specify the order* to *draw said vertices in*
  - Therefore only store **4 vertices** for the **rectangle**
    - Just *specify* the *order* at which to **draw the vertices** in

---

- OpenGL has **element buffer objects** that work like this
  - An **EBO** is a **buffer** just like a **vertex buffer object**
    - this stores **indices** that OpenGL decide what **vertices to draw**
      - This is called **indexed drawing**.

```c++
// specify unique indices
float vertices[] = {
     0.5f,  0.5f, 0.0f,  // top right
     0.5f, -0.5f, 0.0f,  // bottom right
    -0.5f, -0.5f, 0.0f,  // bottom left
    -0.5f,  0.5f, 0.0f   // top left 
};
unsigned int indices[] = {  // note that we start from 0!
    0, 1, 3,   // first triangle
    1, 2, 3    // second triangle
};  
```

- When **using indices**
  - Only need **4 vertices** instead of **6** to create **element buffer object**

```c++
unsigned int EBO;
glGenBuffers(1, &EBO);

glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW); 


glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```

- Similar to **VBO** $\to$ bind the **EBO** and *copy indices* into the **buffer** with **glBufferData**
  - Place those **calls** between a **bind** and an **unbind call** $\to$ although **this time** *we specify* **GL_ELEMENT_ARRAY_BUFFER** as the **buffer type*

```c++
```



- The **vertex array object** keeps track of **element buffer object bindings** 
  - The **last element buffer object** that gets bound while a **VAO** is **bound** $\to$ stored as the **VAO's** *element buffer object*
    - Binding to a **VAO** thus *automatically binds to **that EBO**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210618125231249.png)

> VAO stores **glBindBuffer** *calls* when the target is **GL_ELEMENT_ARRAY_BUFFER**
>
> - This means it **stores** the **unbind calls** so make sure you dont **unbind element array buffer** *before* **unbinding the VAO**
>   - Otherwise it has **no EBO configured.**

```c++
// ..:: Initialization code :: ..
// 1. bind Vertex Array Object
glBindVertexArray(VAO);
// 2. copy our vertices array in a vertex buffer for OpenGL to use
glBindBuffer(GL_ARRAY_BUFFER, VBO);
glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
// 3. copy our index array in a element buffer for OpenGL to use
glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);
// 4. then set the vertex attributes pointers
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);  
  
// ..:: Drawing code (in render loop) :: ..
glUseProgram(shaderProgram);
glBindVertexArray(VAO);
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0)
glBindVertexArray(0);
```

## Additional resources

- [antongerdelan.net/hellotriangle](http://antongerdelan.net/opengl/hellotriangle.html): Anton Gerdelan's take on rendering the first triangle.
- [open.gl/drawing](https://open.gl/drawing): Alexander Overvoorde's take on rendering the first triangle.
- [antongerdelan.net/vertexbuffers](http://antongerdelan.net/opengl/vertexbuffers.html): some extra insights into vertex buffer objects.
- [learnopengl.com/In-Practice/Debugging](https://learnopengl.com/In-Practice/Debugging): there are a lot of steps involved in this chapter; if you're stuck it may be worthwhile to read a bit on debugging in OpenGL (up until the debug output section).

