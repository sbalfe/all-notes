# Colours

- Colors  cant **take any color value** with each **object** possessing its **own colour**
  - Must map digitally the **infinite real colours** to **limited digital values**
    - not every colour can **represented digitally**
- use **RGB** values
  - Use various combinations in the **range** of **[0,1]** 
    - Represent **almost any color** there is
      - Example $\to$ a **coral color** $\to$ define a **color vector** as :

```c++
glm::vec3 coral(1.0f, 0.5f, 0.31f);   
```

- The colour it is , is what it **reflects** of course as with **real life**
  - Colors that are **not absorbed** are the colours we **see**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210727212943838.png)

- white light $\to$ collection of **all visible colours**
  - The object **absorbs** $\to$ a *large portion* of **those colors**
    - only reflects colors that **represent** the *objects color* and the **combination** of those is **what we perceive**.
      - In this case $\to$​ **a coral color** 
- More complex really $\to$ look at **PBR** later

---

- Define a **light source** within **OpenGL**
  - Want to give this **source** a **color** 
    - Give it some `white` color like **previous example** 
- If you **multiply** the *light source color* with some **objects color value**
  - The *resulting* color would be the **reflected** color of the **object** and **therefore** its **perceived color**

---

```c++
/* define the color of our light source */
glm::vec3 lightColor(1.0f, 1.0f, 1.0f);

/* define the color of our toy */
glm::vec3 toyColor(1.0f, 0.5f, 0.31f);

/* multiplication of them together obtains the colour its perceived as*/
glm::vec3 result = lightColor * toyColor; // = (1.0f, 0.5f, 0.31f);
```

- toy absorbs a **large portion of white light**
  - Reflects *several red / green / blue values* based on **its own colour value**
    - This is **representation** of *how colors* would **work** in the **real world**
- Define an **objects color** as the *ammount of each component if reflects from a* **light source**
  - What happens if you use **green light**

```c++
glm::vec3 lightColor(0.0f, 1.0f, 0.0f);
glm::vec3 toyColor(1.0f, 0.5f, 0.31f);
glm::vec3 result = lightColor * toyColor; // = (0.0f, 0.5f, 0.0f);
```

- The toy has **no red and blue** light to **absorb and / or reflect**
  - The toy absorbs **half** of the **light green value** 
    - Also **reflects half however**
      - Appears as a **dark greenish colour**
- If we use **green light** $\to$ only the **green color** components can **be reflected** and **therefore perceived.**
  - No *red / blue* colors are **perceived**
    - As a result the **coral object** becomes **dark greenish color**
- Final example with **dark olive green light**

```c++
glm::vec3 lightColor(0.33f, 0.42f, 0.18f);
glm::vec3 toyColor(1.0f, 0.5f, 0.31f);
glm::vec3 result = lightColor * toyColor; // = (0.33f, 0.21f, 0.06f);
```

- Obtain **interesting colours** from objects using **different light colors**
  - Not hard to **get creative** with the **colors**
    - Start the scene with the **colours**

## Lighting scene

- First require an **object** to **cast the light** on 
  - use the **container cube** from the **previous chapter**
- Require a **light object** to *show* where the **light source** is *located* in the **3D scene**
  - Simplicity $\to$ represent the **light source** with a *cube also*
    - Already have **vertex data**. 

---

- Filling a **vertex buffer object** / setting **attribute pointer** should be straightforward as of now.

---

- Require a **vertex shader** first to **draw the container**
  - The **vertex positions** of the container are the **same**
    - We *dont need texture coordinates*
- Use a **stripped** down version of the **vertex shader** *previously.*

```c++
#version 330 core
layout (location = 0) in vec3 aPos;

uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

void main()
{
    gl_Position = projection * view * model * vec4(aPos, 1.0);
} 
```

- Update the **vertex data** and **attribute pointers** to match this **vertex shader**
  - if you want $\to$ can **keep texture** data and **attribute pointers** , just not using them at the moment

---

- Render a **light source cube**
  - Generate a **new VAO** specifically for **the light source**
    - Could render **the light source** with the **same VAO** and do a **couple light positions** transformations on the **model matrix**
- Upcoming chapters $\to$ *change* the **vertex data** and **attribute pointers** of the **container object** often
  - Do not want **these changes** to **propagate** to the **light source object**
    - Only care about **light cube** *vertex position*
- Create a **new VAO**

```c++
unsigned int lightVAO;
glGenVertexArrays(1, &lightVAO);
glBindVertexArray(lightVAO);

// we only need to bind to the VBO, the container's VBO's data already contains the data.
glBindBuffer(GL_ARRAY_BUFFER, VBO);

// set the vertex attribute 
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);
```

- The code is straightforward
- Create the container and the light source cube
  - One final thing is to define and that **is the fragment shader**  for the **container / light source**

```c++
#version 330 core
out vec4 FragColor;
  
uniform vec3 objectColor;
uniform vec3 lightColor;

void main()
{
    FragColor = vec4(lightColor * objectColor, 1.0);
}
```

- Accepts both an **object color** and a **light color** from a **uniform variable**
  - Multiply the **lights color** with the **objects reflected color** like discussed at the **start** of the **chapter**

- Shader should be **simple to understand**

```c++
// don't forget to use the corresponding shader program first (to set the uniform)
lightingShader.use();
lightingShader.setVec3("objectColor", 1.0f, 0.5f, 0.31f);
lightingShader.setVec3("lightColor",  1.0f, 1.0f, 1.0f);
```

- When we **start to update** these *lighting shaders* in
  - The light source cube is **affected also** , unlike what we want
  - Do **not want** the **light source objects** color to be *affected by lighting* calculations
    - Rather just **keep** the **light source** *isolated* from the **rest**
      - Want the **light source** to have **constant bright color** and **unaffected by** by the **other colors**

- Create a **second set of shaders** to draw the **light source cube**
  - Therefore being **safe** from *any changes* to the **lighting shaders**
    - The *vertex shader* is the same as the **lighting vertex shader** so you can **copy source code over**

- Fragment shader of the **light source cube** ensures the **light source cube remains bright** defining a **white color on the lamp**

```c++
#version 330 core
out vec4 FragColor;

void main()
{
    FragColor = vec4(1.0); // set all 4 vector values to 1.0
}
```

- Render the **container object** / others
  - using the **lighting shader** just defined
    - When you wish to **draw** the **light source** $\to$​ use the **light source shaders**
- During lighting chapters $\to$ gradually update the **lighting shaders** to achieve *more realistic results*

---

- Purpose of **light source cube** is to **show** where the **light comes from**
  - Often define a **light source position** somewhere in the scene, this is just a **position** , with no visual meaning
- To **show** where the light source is we **render a cube** at the **same location** of the **light source**
  - Render this with the **light source cube shader** to ensure it stays **white** regardless conditions of the scene.

- Lets **declare** a *global **vec3*** variable that **represents** the **light source location** in the *world space coordinates* 

```c++
glm::vec3 lightPos(1.2f, 1.0f, 2.0f);
```

- Translate the **light source cube** to the **light source** 

```c++
model = glm::mat4(1.0f);
model = glm::translate(model, lightPos);
model = glm::scale(model, glm::vec3(0.2f)); 
```

- The **resulting render code** for the **light source cube** should then *look something like this*:

```c++
lightCubeShader.use();
// set the model, view and projection matrix uniforms
[...]
// draw the light cube object
glBindVertexArray(lightCubeVAO);
glDrawArrays(GL_TRIANGLES, 0, 36);
```

- Injection **all the code fragments** at their **appropriate locations** result in **clean OpenGL** application properly 
  - Configured for **experimenting** with **lighting**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210728015144854.png)

- 

