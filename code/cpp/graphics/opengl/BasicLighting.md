# Basic Lighting

- **Phong lighting model**
  - Combination of *ambient / diffuse / specular* 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210728174629487.png)

- Ambient > even if dark there is still light such as in the moon or distant lights
- Diffuse > simulates **directional impact** of light object hitting an object. most significant feature of lighting model, the more a part of an object face light  > brighter
- Specular > simulates bright spot of light appearing on shiny objects > specular highlights are more **inclined to color**

---

- Create **visually interesting scenes** 

  - Must *simulate* the **3 lighting components**

     

### Ambient lighting

- Light arrives from many directions and source of course.
- Light scatters and bounces > reaches many directions
- Light therefore **reflects** on surfaces > has direct impact on lighting of an object
- Algorithms that take this into consideration are **global illumination** algorithms

---

- We *are not big fans* of **complicated / expensive algorithms** 
  - Start using simple model of illumination called **ambient lighting**.
- Previously we used a **small constant light** color that adds to the **final resulting color** of the objects fragments
  - Appears as if **always scattered light** even if *not direct light source*
- Add ambient lighting by taking light color and multiply by an ambient strength value
  - Multiply this by the object color.
  - Use this as the frag color in the cube object shader.

```c++

void main()
{
    float ambientStrength = 0.1;
    vec3 ambient = ambientStrength * lightColor;

    vec3 result = ambient * objectColor;
    FragColor = vec4(result, 1.0);
}  
```

- object now appears dark, but not fully as there is still some ambient lighting present
- Unaffected light cube as a different shader is applied to that.

## Diffuse Lighting

- Gives objects more brightness the **closer** the **fragments** are aligned to the light rays from a light source

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210728181242141.png)

- Light source on left , ray targeted at single fragment of the object.
- Must **measure the angle** the light ray touches the fragment.
- If **perpendicular** to the object surface it has **greatest impact** 
- Measure angle between light and fragment using a **normal vector**
- This is a vector **perpendicular** to the **fragments surface** shown in *yellow*
- Angle between two vectors calculated with **dot product**

---

- Lower angle between two unit vectors > the more the dot product inclined towards 1
- Angle between both vectors is 90 degrees > dot becomes 0
- Same applies to $\theta$  > **large** the $\theta$ > **less of impact** the light has on fragments color

---

> Note that to get (only) the cosine of the angle between both vectors we will work with *unit vectors* (vectors of length `1`) so we need to make sure all the vectors are normalized, otherwise the dot product returns more than just the cosine (see [Transformations](https://learnopengl.com/Getting-started/Transformations)).

- Dot product returns **scalar** used to **calculate lights impact** on the fragments color
- Gives us differently lit fragments based on **their orientation** towards the **light**
- To calculate diffuse lighting:
  1. **NORMAL VECTOR** 
  2. **DIRECTED LIGHT RAY** - the difference vector between the lights position and the fragment position
     - Calculate this light ray > must obtain **light position vector** and **fragments position vector**

### Normal vectors

- Vertex on its own has **no surface**, just a single point in space
- retrieve normal vector by using **surrounding vertices** to figure out the **surface** of the *vertex* 

- trick to calculate normal vector for **all cube vertices** via using the **cross product** 
  - 3D cube not too complicated therefore **add manually** to the **vertex data**
- Visualise the **normals** are *vectors perpendicular* to each **planes surface** , a cube has 6 planes

---

- Take this normal in as an extra attribute 

```glsl
#version 330 core
layout (location = 0) in vec3 aPos;
layout (location = 1) in vec3 aNormal;
...
```

- Added a ==**normal vector**== to *each* of the **vertices** and updated the **vertex shader**.
- Update attribute pointers here.
- Light source cube uses **same vertex array** for vertex data 
- Lamp shader has no newly added normal vectors
  - Do not have to update the lamp shaders attribute configurations
- Must modify the vertex attribute pointers to reflect the **new vertex array size** 

```c++
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void*)0);
glEnableVertexAttribArray(0);
```

- Only want to use **first 3 floats** of *each vertex* and **ignore** the **last 3 floats**
  - Therefore only update the **stride** parameter to **6 * sizeof(float)** then we *are done*

>It may look inefficient using **vertex data that is not completely used** by the lamp shader, but the vertex data is already stored in the GPU's memory from the container object so we **don't have to store new data into the GPU's memory**. This actually makes it **more efficient** compared to allocating a new VBO specifically for the lamp.

- All the **lighting calculations** are done in the **fragment shader** , move to **normal vectors** from the **vertex shader** to **fragment shader**

---

```c++
out vec3 Normal;

void main()
{
    gl_Position = projection * view * model * vec4(aPos, 1.0);
    Normal = aNormal;
}
```

- **Declare** the *corresponding input variable* in the **fragment shader**

```glsl
uniform vec3 lightPos;  
```

### Diffuse color calculation

- Have the **normal vector** for **each vertex**
- Need the light **position vector** and the **fragment position vector**
- As the **light position** is a single static variable, we can **declare** as *uniform* in the **fragment shader**

```glsl
uniform vec3 lightPos;  
```

- Update the uniform in the render loop (outside as it doesnt change in the frames)
- use **lightPos** vector declared in **previous chapter** as the location of the **diffuse light source**

```c++
lightingShader.setVec3("lightPos", lightPos);  
```

- Last thing is the **actual fragment position**
- We perform all light calculations in the **world space** therefore we want a **vertex position** that is in the world space
- Accomplish this by **multiplying** the **vertex position attribute** with the **model matrix** only
- Done in the **vertex shader**

```c++
out vec3 FragPos;  
out vec3 Normal;
  
void main()
{
    gl_Position = projection * view * model * vec4(aPos, 1.0);
    FragPos = vec3(model * vec4(aPos, 1.0));
    Normal = aNormal;
}
```

- Lastly add **corresponding** input variable to the **frag shader**

```glsl
in vec3 FragPos;
```

- The **in variable** is **interpolated** from the **3 world position vectors** of *the triangle* 
  - Forms **FragPos** *vector* that is **per fragment world position**
- All required variables are set $\to$ start **lighting calculations** 

---

- Must calculate **direction vector** between light source / fragment position.
- Previous section $\to$ know the **lights direction vector** is the *difference vector* between the **lights position vector** and the **fragments position vector**
- Recall from **transformations chapter**
  - Can easily *calculate* the **difference** by *subtracting* both vectors from **each other**
- Make sure all the **relevant vectors** end up as **unit vectors** so we **normalize** both the **normal** and the **resulting direction vector.**

```c++
vec3 norm = normalize(Normal);
vec3 lightDir = normalize(lightPos - FragPos);  
```

> - When calculating lighting we usually **do not care about the magnitude** of a vector or their position; we only care about their direction. 
> - Because we only care about their **direction almost all the calculations are done** with **unit vectors** since it **simplifies most calculations** (like the dot product).
> -  So when doing lighting calculations, make sure you always **normalize the relevant vectors** to ensure **they're actual unit vectors**. Forgetting to normalize a vector is a popular mistake.

- Must calculate the **diffuse impact** of **light** on the current fragment by **taking dot product** between *norm / lightDir* vectors.
- Resulting value **is then multiplied** with the **lights color** to obtain the **diffuse component** resulting in a **darker diffuse component** the *greater* the **angle** between **both vectors**

```c++
float diff = max(dot(norm, lightDir), 0.0);
vec3 diffuse = diff * lightColor;
```

- If the **angle** between both vectors **> 90 degrees** 
- The result of the **dot product** becomes **negative** and end **with negative diffuse component**
- Therefore we use **max function** to return the **highest** of **both** the parameters to make sure the **diffuse component**
  - And therefore the colors
  - Never become negative.
- Lighting for **negative colors** is *not defined* so you must stay away from this
  - Unless for tests.

---

- We now apply our **ambient / diffuse** 
  - Add both of the colors to one another then **multiply the result** with the color of the **object** to get the **resulting fragments output color**

```glsl
vec3 result = (ambient + diffuse) * objectColor;
FragColor = vec4(result, 1.0);
```

- If your **application / shaders** compiled succesfully you should see this

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210728213612660.png)

- normal passed from vertex shader to fragment shader
- Calculations in the fragment shader are **all done in world space** therefore we should **not transform the normal vectors** to world space coordinates as well
- Yes but we cannot just multiply by **model matrix**

---

- Normal vectors are just **direction** and not a specific position in the space
- Normal vectors do **not have homogenous coordinates** 
  -  Therefore the translation has **no effect** 
- To multiy normal vectors with **model matrix**, must remove the translation part of the matrix by taking the upper left with **3x3 matrix** of the **model matrix**
- Set the w component to 0

---

- If the **model matrix** would performs some **non uniform scale**
- Vertices would be changed in such a way that the **normal vector** is **not perpendicular** to the **surface anymore**
- Following image shows the effect of such a model matrix with **non uniform scaling**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210728221610702.png)

---

- When applying a **non uniform scale**
  - The **normal vectors** are **not perpendicular** to the **corresponding** surface anymore which **distorts** the **lighting**
- Trick of fixing this is **to use different model matrix** for normal vectors
- Called the **normal matrix** which uses some **linear algebraic operations** to remove the effect of **wrongly scaling the normal vectors**

http://www.lighthouse3d.com/tutorials/glsl-12-tutorial/the-normal-matrix/ - explanation of normal matrix derivation.

---

- Normal matrix is defined as the **transpose** of the **inverse** of the **upper left 3 x 3 model matrix**
- Most resources define the normal matrix as derived from **model view** matrix
  - As we are in **world space** $\to$ we derive from the **model matrix** 

---

- In **vertex shader** generate the **normal matrix** using the **inverse / transpose** functions in the **vertex shader** that work on *any matrix type*
  - Cast the **3 x 3 matrix** to ensure it **loses** its **translation properties**
  - Then multiply with the **vec3 normal vector**

```glsl
Normal = mat3(transpose(inverse(model))) * aNormal;  
```

> Inversing matrices is a costly operation for shaders, so wherever possible try to avoid doing inverse operations since they have to be done on each vertex of your scene. For learning purposes this is fine, but for an efficient application you'll likely want to calculate the normal matrix on the CPU and send it to the shaders via a uniform before drawing (just like the model matrix).

- In diffuse lighting section , the lighting was fine as we did no scaling on the object
  - There was no need to use a **normal matrix** 
    - Could have just multiplied the normals with model matrix
      - Doing non uniform scale however $\to$ essential you multiply normal vectors with the **normal matrix**

## Specular Lighting

- Based also on **lights direction vector** and **objects normal vectors**
  - Also based on **view direction**
    - As in the **what direction** is the player **looking at that fragment**
- Specular lighting is based on **reflective properties of surfaces**
  - Think of **object** surface as a **mirror**
    - The **specular lighting** is the **strongest** wherever we see the light reflected on the surface 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210728225952451.png)

- Calculate a **reflection vector** by **reflecting the light direction** around the **normal vector**
  - Calculate the **angular distance** between this **reflection vector** and the **view direction**
- The **closer the angle** , the **greater the impact** of the **specular **light

---

-  The **resulting effect** is that we see a **bit of a highlight** when looking at the **lights direction** reflected via **the surface**
- The **view vector** is the **extra** variable required for **specular lighting** which we **calculate** using **viewers world space** position and **fragments position** 
- Calculate the **specular intensity** , multiply this with the **light color** and *add* to the **ambient / diffuse components** 

---

> - We chose to do the lighting calculations in **world space**, but most people tend to prefer doing **lighting in view space**. 
> - An advantage of **view space** is that the **viewer's position is always at `(0,0,0)`** so you already got the **position of the viewer** for free. 
> - However, I find **calculating lighting in world space more intuitive for learning purposes**. 
> - If you still want to **calculate lighting in view space** you want to **transform all the relevant vectors** with the **view matrix** as well (don't forget to **change the normal matrix too**).

- Obtain **world space coordinates** of the **viewer** $\to$ take the 

https://ogldev.org/www/tutorial19/tutorial19.html

