# Textures

https://www.learnopengles.com/tag/texture-mapping/

http://www.c-jump.com/bcc/common/Talk3/OpenGL/Wk07_texture/Wk07_texture.html

https://open.gl/textures

- Limit to how **much** a **colour** can be used to **create interesting images** , kind of, just a **fuck ton of vertices** must be specified.
  - Takes up **serious overhead** as each model need **lots more vertices** 
    - For *every vertex* a **colour attribute** as well.
- Prefer to use **texture**
  - This is a **2D image** (1D / 3D exist) to add **detail** to **some object**
    - Like a **piece of paper** with a **brick image** example on **neatly folder** over **3D house** so it looks **like** your **house** has a **stone exterior**
- Insert a **lot of detail** into **one image**
  - Give the **illusion** the *object* **extremely detailed** without having to **specify extra vertices**.

---

> Textures can store **collection** of **arbitrary data** to **send to the shaders** (*later*)

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210618231821054.png)

- To **map** some **texture** $\to$ triangle
  - must **tell** each *vertex* of the **triangle** which **part** of the **texture** corresponds to **each vertex.**
    - Therefore a **texture coordinate** specified with **them** that *specifies* which **part** of the **texture image** to *sample from*.
      - **Fragment interpolation** $\to$ does the **rest** for the **other fragments**.
- Texture *coordinates* $\to$ range from $0$ to $1$ in the **x / y axis**
  - Remember $\to$ use **2D texture images**
    - Retrieving the **texture color** using **texture coordinates** is ==sampling== 
- Texture coordinates start at **(0,0)** for the **lower left** *corner* of a **texture** *image* to **(1,1)**
  - For the **upper right corner** of the **texture image**
    - Following image **shows** how you **map texture coordinates** to the **triangle**:

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210618232725879.png)

- Specify **3 texture coordinate points** for the **triangle**
  - We want the **bottom left side** of the *triangle* to **correspond** with the **bottom left** side of the **texture** so use **(0,0)** texture **coordinate** *triangle bottom left vertex*.
- Same **applies** to the **bottom-right side** with (1,0) texture **coordinate** 
  - The **top of the triangle** should **correspond** with the **top center** of the **texture** therefore take **(0.5, 1.0)**
    - We **only have** to pass **3 texture coordinates** to the **vertex shader** $\to$ passes those to **the fragment shader** that *neatly interpolates* every **texture coordinate** for **every fragment**
- Resulting **texture** coordinates would **look like this**

---

```c++
float texCoords[] = {
    0.0f, 0.0f,  // lower-left corner  
    1.0f, 0.0f,  // lower-right corner
    0.5f, 1.0f   // top-center corner
};
```

- Texture sampling has **loose interpretation**
  - Completed in **various ways**
    - Our job to **tell OpenGL** how it should **sample its textures**.

## Texture Wrapping

- Texture coordinates **range from** $(0,0)$ $\to$ $(1,1)$
  - What occurs when **specifying coordinates** that are *outside this range*
    - The **default behaviour** of **OpenGL** is to **repeat texture images** $\to$ ignore **integer part** of **floating point texture coordinate**.
- More options
- ==GL_REPEAT==: The default behaviour for textures. **Repeats the texture image**.
- ==GL_MIRRORED_REPEAT==: Same as **GL_REPEAT** but **mirrors the image with each repeat**.
- ==GL_CLAMP_TO_EDGE==: **Clamps** the coordinates **between `0` and `1`**. The result is that **higher coordinates** become **clamped to the edge**, resulting in a **stretched edge pattern.**
- ==GL_CLAMP_TO_BORDER==: Coordinates **outside the range are now** given a **user-specified border color**



![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210619011131823.png)

---

- Each of these can be **set per coordinate** axis
  - $s$ ,$t$ and $r$ (3D ONLY) equivalent to **x,y,z**.
    - With the **glTexParamater*** function

---

```c++
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_MIRRORED_REPEAT);// x 
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_MIRRORED_REPEAT);// y
```

- arg1 > specifies the **texture target**, working with **2D** therefore set to **2D**
- arg2 > tells us **what option** we want to set for **which texture axis**
  - Configure for **both S , T** axis.
- arg3 > pass in a **texture wrapping mode** and this case we wish for **repeat**

---

- Choosing **GL_CLAMP_TO_BORDER** $\to$ specify a **border color**
  - Done using **fv equivalent** of `glTexParameter`  with **GL_TEXTURE_BORDER_COLOR** as option to pass in a  **float array** of the **border's color value**

```c++
float borderColor[] = { 1.0f, 1.0f, 0.0f, 1.0f };
glTexParameterfv(GL_TEXTURE_2D, GL_TEXTURE_BORDER_COLOR, borderColor);  
```

## Texture Filtering

https://stackoverflow.com/questions/58550323/why-do-we-need-texture-filtering-in-opengl

- Texture coordinates **do not depend** on *resolution* but can **be any floating point value**
  - Therefore **OpenGL** has to *figure out* which **texture pixel** (known as **texel**) to *map* the **texture coordinate to**.
    - This is **important** if you have a **large object** with **low resolution texture**
- There are **options** for **texture filtering**
  - Most vital are ==GL_NEAREST== and ==GL_LINEAR== 

### GL_NEAREST

- aka $\to$ **nearest neighbour** or **point** *filtering* is the **default** texture filtering method in OpenGL.
  - When set to this $\to$ OpenGL selects the **texel** that *center* is **closest** **to the texture coordinate**
    - You can see **4 pixels** $\to$ cross represents the **exact texture coordinate**
      - *upper left* texel has its **center closest** to the **texture coordinate** therefore is the **sampled color**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210619003458751.png)

### GL_LINEAR

- Known as **bilinear filtering**
  - Takes some **interpolated value** from the **texture coordinates** *neighboring texels*.
    - Approximating a **color** between the **texels**

- The **smaller** the **distance** from the **texture coordinate** to a *texel center*
  - The *more* that **texel's** color **contributes** to the **sample color**
    - below is a **mixed color** of the **neighbouring pixels** is *returned*.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210619004052419.png)

---

- But what is **the visual effect** of such **texture filtering method**
  - Lets see **how these methods** work when **using a texture** with a **low resolution** on a **large object**
    - Texture is **therefore scaled upwards** and **individual texels** are **noticeable**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210619004605389.png)

- `GL_NEAREST` results in **blocked patterns** where we can **clearly** see the **pixels** that *form the texture* while `GL_LINEAR`
  - Produces a **smoother pattern** where the **individual pixels** are *less visible*
- `GL_LINEAR` produces **more realism** but some may prefer **8 bit look** therefore pick the `GL_NEAREST` option.

---

- Texture filtering $\to$ set for **magnifying / minifying operations** (scaling up / downwards)
  - can use **nearest neighbour filtering** when the *textures* are *scaled downwards* and **linear filtering** for **upscaled textures**
    - Therefore must **specify** the *filtering method* for **both options** via `glTexParameter* `

```c++
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
```

## Mipmaps

- Picture some **large room** with **thousands of objects**
  - Each one has an **attached texture**
    - There are *objects* that are **far away** and have the **same high resolution** *texture* attached as the **objects** that are *close the viewer*
- The further away objects produce only a **few fragments** $\to$ OpenGL has **difficulty** retrieving the **right color value** for the fragment from the **high resolution texture**
  - As it must **select a texture color** for some *fragment* that spans a **large part of the texture**
    - Produces **visible artifacts** on *small objects* 
    - Also **wastes memory bandwidth** using **high res** on **small objects**

---

- Solve using **mipmaps**
  - A *collection* of **texture images** where every **subsequent texture** is **twice as small** to the **previous one**
    - Idea:
      1. After a **certain distance threshold** from the *viewer* $\to$ use a **different mipmap texture** that *best suits the distance to the object*
      2. As the object is **far away** $\to$ the **smaller resolution** is *less noticeable* to the **user**.

- OpenGL able to **sample** the *correct texels* 
  - With **less cache memory** involved when **sampling** that part of the **mipmaps**
    - Closer look at a **mipmapped texture**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210619024857841.png)

- Use `glGenerateMipMaps` to do all the work for us after **creation of the texture**

---

https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/glTexParameter.xhtml

- When **switching** between the *mipmaps levels* during **rendering OpenGL**
  - May show **artifacts** such as **sharp edges** visible between **two mipmap layers**
    - Like **normal texture filtering** $\to$ possible to **filter** between **mipmap** levels using **NEAREST / LINEAR** filtering for **switching** between *mipmap levels*.
- To **specify** the **filtering method** between *mipmap levels*
  - Replace the **original filtering methods** with one of the **following four options**

https://paroj.github.io/gltut/Texturing/Tut15%20Needs%20More%20Pictures.html

>- ==GL_NEAREST_MIPMAP_NEAREST==: takes the **nearest mipmap** to **match the pixel size** and **uses nearest neighbour interpolation** for texture sampling.
>- ==GL_LINEAR_MIPMAP_NEAREST==: takes the **nearest mipmap level** and samples that level **using linear interpolation**.
>- ==GL_NEAREST_MIPMAP_LINEAR==: **linearly interpolates between the two mipmaps** that most **closely match the size of a pixel** and **samples** the **interpolated level** via nearest **neighbour interpolation.**
>- ==GL_LINEAR_MIPMAP_LINEAR==: **linearly interpolates** between the **two closest mipmaps** and **samples** the **interpolated** level **via linear** **interpolation**.

- Just **like texture filtering**
  - Can set the **filtering method** to *one of the **4 above***

```
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
```

- Common **mistake** is to **set** one of the **mipmaps** *filtering options* as the **magnification filter**
  - This does **not have any effect** as **mipmaps** are used for when **textures** are **downscaled**
    - Texture **magnification** does **not use mipmaps** and *giving it a **mipmap filtering option*** will *generate* an **OpenGL** *GL_INVALID_ENUM* *error code*

## Loading and creating textures

- Hard to load images sometimes such as custom PNG loader.
  - use stb_image.h

### stb_image.h

- single **header** for *image loading library* load most formats and easy to integrate.

```c++
#define STB_IMAGE_IMPLEMENTATION
#include "stb_image.h"
```

- By defining **STB_IMAGE_IMPLEMENTATION**
  - This **pre-processor** *modifies* the **header file** so it only **contains** the *relevant source code*
    - Turning the **header file** into a **.cpp** file

---

- For following texture sections $\to$ use **wooden container image.**
  - To *load* an image using **stb_image.h** use **stbi_load**

```c++
int width, height, nrChannels;
unsigned char *data = stbi_load("container.jpg", &width, &height, &nrChannels, 0); 
```

## Generating a texture

- Like any **previous objects** in *OpenGL*
  - Textures are **referenced** with some **ID**, create one as such.

```c++
unsigned int texture;
glGenTextures(1, &texture);

glBindTexture(GL_TEXTURE_2D, texture);  

glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
glGenerateMipmap(GL_TEXTURE_2D);
```

- When `glTexImage2D` is called
  - Currently **bound texture object** $\to$ now has the **texture image attached to it**
- Has only the **base level** of the **texture image loaded**
  - If we **wish** to use **mipmaps**  $\to$ must **specify** all the **images manually** $\to$ via *incrementing* the **second argument**
    - Alternatively call `glGenerateMipmap` after **generating the texture**.
      - Automatically generates the **require mipmaps** for the *currently bound texture*.

- Good to **free the memory of the image** after the bind has occurred:

```c++
stbi_image_free(data);
```

- Whole process of **generating a texture** looks something like this:

```c++
unsigned int texture;
glGenTextures(1, &texture);
glBindTexture(GL_TEXTURE_2D, texture);
// set the texture wrapping/filtering options (on the currently bound texture object)
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_REPEAT);	
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_REPEAT);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
// load and generate the texture
int width, height, nrChannels;
unsigned char *data = stbi_load("container.jpg", &width, &height, &nrChannels, 0);
if (data)
{
    glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
    glGenerateMipmap(GL_TEXTURE_2D);
}
else
{
    std::cout << "Failed to load texture" << std::endl;
}
stbi_image_free(data);
```

## Applying textures

- Use the **rectangle shape** drawn with **glDrawElements**
  - need to inform **OpenGL** how to *sample* the **texture** so *we will* have **the vertex data** with the **texture coordinates**

```c++
float vertices[] = {
    // positions          // colors           // texture coords
     0.5f,  0.5f, 0.0f,   1.0f, 0.0f, 0.0f,   1.0f, 1.0f,   // top right
     0.5f, -0.5f, 0.0f,   0.0f, 1.0f, 0.0f,   1.0f, 0.0f,   // bottom right
    -0.5f, -0.5f, 0.0f,   0.0f, 0.0f, 1.0f,   0.0f, 0.0f,   // bottom left
    -0.5f,  0.5f, 0.0f,   1.0f, 1.0f, 0.0f,   0.0f, 1.0f    // top left 
};
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210619183019335.png)

```c++
glVertexAttribPointer(2, 2, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void*)(6 * sizeof(float)));
glEnableVertexAttribArray(2); 
```

- Adjust stride as there are more attributes to pass over.
- add a texture coordinate import to the **vertex shader** 

```c++
#version 330 core
layout (location = 0) in vec3 aPos;
layout (location = 1) in vec3 aColor;
layout (location = 2) in vec2 aTexCoord;

out vec3 ourColor;
out vec2 TexCoord;

void main()
{
    gl_Position = vec4(aPos, 1.0);
    ourColor = aColor;
    TexCoord = aTexCoord;
}
```

- The **fragment shader** then accepts the **TexCoord** *output variable* as an **input variable**
  - The **fragment shader** also have **access** to the **texture object**
    - How do we **pass the texture object** to the **fragment** *shader*
- GLSL built in **data type** for *texture objects* called a **sampler**
  - Takes as **postfix** $\to$ the **texture type** we want
    1. sampler1D
    2. sampler2D
    3. sampler3D
- Then add a **texture** to the **fragment** *shader* by **declaring** a *uniform sampler2D* 
  - Later assign the texture to.

```c++
#version 330 core
out vec4 FragColor;
  
in vec3 ourColor;
in vec2 TexCoord;

uniform sampler2D ourTexture;

void main()
{
    FragColor = texture(ourTexture, TexCoord);
}
```

- To **sample** the color of a **texture** use **GLSL** *built in **texture*** function that **takes** as its **first argument** a *texture sampler*
  - The **second argument** is **corresponding** *texture coordinates* 
    - The **texture** function then *samples* the **corresponding** *color* using the **texture parameters** that we **set earlier**
- The **output** of this **fragment shader** is then the **filtered color** of the **texture** at the *interpolated texture coordinate*.

---

- All we **do now** is just **bind the texture** before calling **glDrawElements**
  - automatically assign the **texture** to the **fragment shader's sampler**

```c++
glBindTexture(GL_TEXTURE_2D, texture);
glBindVertexArray(VAO);
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```

- Can make it multicoloured by applying the **color vertices** as a multiplication to the original texture

---

## Texture Units

- The **sampler2D** variable is **uniform**, however we **didn't assign it** with *some value* with **glUniform** via use of **glUniform1i**
  - can assign a **location value** to the **texture sampler** so we *can set multiple textures* at **once** in a **fragment shader**
- The **location** of a **texture** is *more commonly* known as **texture unit.**
  - The **default** *texture unit* for a **texture** is $0$ $\to$ the **default active texture unit.**
    - Therefore we **dont need** to *assign a location* for the **previous section**.

- All **graphics drivers** assign some **default texture unit** so the **previous section** may *not have rendered* for you.

---

- Main **purpose** of **texture units** is to *allow* us to **use more than texture 1** in our **shaders**
  - Via assigning **texture units** to the **samplers**
    - Can **bind** to **multiple textures** at once as long as you **activate** the *corresponding texture unit* first
- Like `glBindTexture` $\to$ can activate **texture units** using `glActiveTexture` passing **in the texture unit** that we wish to use
  - pass in the texture we **wish to use**

```c++
glActiveTexture(GL_TEXTURE0); // activate the texture unit first before binding texture
glBindTexture(GL_TEXTURE_2D, texture);
```

- After **activating** a **texture unit**
  - Subsequently **call** `glBindTexture` $\to$ bind that **texture** to the **currently active texture unit**
    - Texture unit **GL_TEXTURE0** is always **default activated**.
      - So we **didn't have** to *activate* any **texture units** in the **previous example** when using `glBindTexture` 

> - OpenGL should have a at **least a minimum of 16 texture units** for you to use **which you can activate** using **GL_TEXTURE0** to **GL_TEXTURE15**. 
> - They are **defined in order** so we could also **get GL_TEXTURE8** via **GL_TEXTURE0 + 8** for example, which is **useful when we'd have to loop** over **several texture units**.

- Must still edit the **fragment shader** to *accept* **another sampler**
  - Should be pretty **straightforward now**

```c++
#version 330 core

uniform sampler2D texture1;
uniform sampler2D texture2;

void main()
{
    FragColor = mix(texture(texture1, TexCoord), texture(texture2, TexCoord), 0.2);
}
```

- The **final output color** is **now the combination** of *two texture lookups* 
  - GLSL built in **mix function** takes **two values** as *input* and then **linear interpolates** between them $\to$ based on **third argument**
- If the **third value** is $0.0$ returns the **first output**, if 1 then **the second value**
  - Value 0.2 therefore **80%** of the **first input** resulting in a **80/20 mix**

---

- Now want to **load and create** the *other texture*
  - Should be **familiar** with said **steps**
    - Make sure to **create** *another texture object* , then **load the image** and **generate** *the final texture* using **glTexImage2D**.

- Second texture $\to$ use an **image** of your *awesomeface.png*

```c++
unsigned char *data = stbi_load("awesomeface.png", &width, &height, &nrChannels, 0);
if (data)
{
    glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGBA, GL_UNSIGNED_BYTE, data);
    glGenerateMipmap(GL_TEXTURE_2D);
}
```

- We **now load** a *.png* image that includes an **alpha** *transparency channel*
  - Means we **need** to **specify** the **image data** contains an **alpha channel** using **GL_RGBA**
    - Otherwise it **incorrectly** *interpret* the **image data**

- Use the **second texture** and the **first texture** $\to$ must have to **change** the *rendering procedure* by a **bit** by **finding both textures** to the **corresponding** *texture unit*

```c++
glActiveTexture(GL_TEXTURE0);
glBindTexture(GL_TEXTURE_2D, texture1);
glActiveTexture(GL_TEXTURE1);
glBindTexture(GL_TEXTURE_2D, texture2);

glBindVertexArray(VAO);
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0); 
```

- Must tell **OpenGL** to *which texture unit*  that **each shader sampler** belongs to by **setting each sampler** using `glUniform1i` 
  - Only have to **set this once**, do *before the render loop*

```c++
ourShader.use(); // don't forget to activate the shader before setting uniforms!  
glUniform1i(glGetUniformLocation(ourShader.ID, "texture1"), 0); // set it manually
ourShader.setInt("texture2", 1); // or with shader class
  
while(...) 
{
    [...]
}
```

- By setting the **samplers** via `glUniform1i` $\to$ make **sure each uniform sampler** corresponds to the **proper texture unit**
  - Obtain **following result**

- Make sure to flip the image using this command.

```c++
stbi_set_flip_vertically_on_load(true);  
```

