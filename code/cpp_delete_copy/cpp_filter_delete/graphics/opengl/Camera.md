

# Camera

- Previously we used **view matrix** to move **backwards a little**
- Simulate movement with **reverse direction**

---

## Camera/View Space

- View Mat > transforms **world** > **view coordinates**
  - This is relative to the **camera position and origin**

- To define a camera:
  1. Require camera **world space position**
  2. The **direction** its **looking at** 
     - A Vector to the **right** / **upwards** from the **camera.**
- We create a **coordinate system** with *3 perpendicular* unit axes 
  - Where **cameras position** is the **origin**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210722000821168.png)

### Camera Position

- This is a **vector in world space**

```c++
glm::vec3 cameraPos = glm::vec3(0.0f, 0.0f, 3.0f);  
```

- Move along Z axis to **go backwards**

### Camera Direction

- The direction its **pointing at**
  - Let the **camera point** to *origin* of *our scene* $(0,0,0)$
    - If you **subtract two vectors** from one another $\to$ the **difference**
- Subtracting the **camera pos** from **scenes origin vector** therefore is the **direction vector** we want.
- For the **view matrix** coordinate system $\to$ we need the **z axis** to be **positive**
  - By convention in **OpenGL** $\to$ the *camera* points **towards** the **-Z axis**
- If we switch **subtracting** order we obtain a **vector** pointing toward **camera positive Z axis.**

```c++
glm::vec3 cameraTarget = glm::vec3(0.0f, 0.0f, 0.0f);
glm::vec3 cameraDirection = glm::normalize(cameraPos - cameraTarget);
```

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210725052836002.png)

> Direction vector is **not the best chosen** name
>
> It is pointing in the **reverse direction** of *what its targeting*.

### Right axis

- We require a **right vector**
  - This represents the **positive** *x-axis* of the **camera space**
- To obtain the **correct vector** 
  - First specify some **up vector** that points **up** into **world space**
    - perform a **cross product** on **up / direction vector** from *step 2*
      - A **==cross product==** is some vector that is **perpendicular** to *both*
- Obtain a **vector** that points in the **positive** *x-axis* direction
  - If we would **switch cross product over** $\to$ obtain points in the **negative x-axis**

```c++
glm::vec3 up = glm::vec3(0.0f, 1.0f, 0.0f); 
glm::vec3 cameraRight = glm::normalize(glm::cross(up, cameraDirection));
```

### Up axis

- Have **both** *x-axis* vector and **z axis vector** 

  - Retrieving the **vector** that points to the **cameras** *positive* *y axis* is easy

    - Take **cross product** of the **right / direction vector**

  ```c++
  glm::vec3 cameraUp = glm::cross(cameraDirection, cameraRight);
  ```

- Help of **cross products** and **vectors**
  - Form the **view/ camera space**
- This is the **gran Schmidt process**

https://www.khanacademy.org/math/linear-algebra/alternate-bases/orthonormal-basis/v/linear-algebra-the-gram-schmidt-process

https://www.youtube.com/watch?v=zHbfZWZJTGc

### Look At

https://www.youtube.com/watch?v=sguGBjYowdc&list=PLqCJpWy5Fohe8ucwhksiv9hTF5sfid8lA&index=23

- If we define a coordinate space using **3 perpendicular** / **non linear** axes
  - create a matrix with **those 3 axes** + **translation vector**
    - Transform **any vector** to this **coordinate space** via **multiplication** of this **matrix**
- This is what our **look at** matrix is doing 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210725074157085.png)

- R , U, D for right / up / down respectively.
- Left matrix and right matrix are inverted (transposed and negated)
  - Since we wish to **translate** the world in the **opposite direction** of where we want the **camera to move**
    - use *look at* matrix $\to$ the **view matrix** just **transforms** all the **world coordinates** to the view space we defined.

- GLM does this for us
  - must specify a camera position / target position and vector that represents what up is in world space

```c++
glm::mat4 view;
view = glm::lookAt(glm::vec3(0.0f, 0.0f, 3.0f), // position of camera
  		   glm::vec3(0.0f, 0.0f, 0.0f), //target to look at
  		   glm::vec3(0.0f, 1.0f, 0.0f)); // what is up
```

```c++
const float radius = 10.0f;
float camX = sin(glfwGetTime()) * radius;
float camZ = cos(glfwGetTime()) * radius;
glm::mat4 view;

/* position of vector is determined by the sin / cos radius passing in time, traverses all points in a circle radius 10.*/
view = glm::lookAt(glm::vec3(camX, 0.0, camZ), glm::vec3(0.0, 0.0, 0.0), glm::vec3(0.0, 1.0, 0.0));
```

## Walk Around

- First configure our camera system with variables at the top of the program

```c++ 
glm::vec3 cameraPos   = glm::vec3(0.0f, 0.0f,  3.0f);
glm::vec3 cameraFront = glm::vec3(0.0f, 0.0f, -1.0f);
glm::vec3 cameraUp    = glm::vec3(0.0f, 1.0f,  0.0f);
```

- Look at function thus becomes

```c++
view = glm::lookAt(cameraPos, cameraPos + cameraFront, cameraUp);
```

- Set camera position to previously defined camera position
- Direction is current position + the **direction vector**
  - This ensures that however we move , the camera keeps looking at the target direction 

```c++
void processInput(GLFWwindow *window)
{
    ...
    const float cameraSpeed = 0.05f; // adjust accordingly
    if (glfwGetKey(window, GLFW_KEY_W) == GLFW_PRESS)
        cameraPos += cameraSpeed * cameraFront;
    if (glfwGetKey(window, GLFW_KEY_S) == GLFW_PRESS)
        cameraPos -= cameraSpeed * cameraFront;
    if (glfwGetKey(window, GLFW_KEY_A) == GLFW_PRESS)
        cameraPos -= glm::normalize(glm::cross(cameraFront, cameraUp)) * cameraSpeed;
    if (glfwGetKey(window, GLFW_KEY_D) == GLFW_PRESS)
        cameraPos += glm::normalize(glm::cross(cameraFront, cameraUp)) * cameraSpeed;
}
```

- Press the wasd keys
  - camera position updated accordingly
    - To move forward / back $\to$ add / subtract values accordingly
  - to move sideways $\to$ do a cross product to create a right vector and move along this vector as so
    - Creates the **strafe vector**

> Note that we normalize the resulting *right* vector. If we wouldn't normalize this vector, the resulting cross product may return differently sized vectors based on the camera Front variable. If we would not normalize the vector we would move slow or fast based on the camera's orientation instead of at a consistent movement speed.

- By now you should be able to move the **camera somewhat**

## Movement speed

- Different machines process frames differently.
  - some are faster than others
    - Must be able to normalize this.
- When users renders more frames another use he calls processInput
  - More often
    - result is that some people move fast / slow depending on the rig
- When shipping an application.
  - make it run on multiple hardware.

---

- Use `deltatime` variable stores the **time** it took to **render** the **last frame**
  - Then **multiply** all *velocities* with the **deltaTime** value.
    - Result is having **large delta Time** in a frame $\to$ meaning the **last frame** took *longer than average*
      - The **velocity** of that frame will be **slightly higher** to balance it all out.
- Calculate **deltaTime** , keep track of the **2 global variables**

```c++
float deltaTime = 0.0f;	// Time between current frame and last frame
float lastFrame = 0.0f; // Time of last frame
```

- In each frame $\to$ calculate **new *deltaTime*** for later use.

```c++
float currentFrame = glfwGetTime();
deltaTime = currentFrame - lastFrame;
lastFrame = currentFrame;  
```

- Now consider this during our **velocity calculations**

```c++
void processInput(GLFWwindow *window)
{
    float cameraSpeed = 2.5f * deltaTime;
    [...]
}
```

- As we are using **deltaTime** 
  - The camera moves at a **constant** speed of **2.5 units per second**
    - Produces a smoother and more consistent camera system for moving around the scene.

## Look Around

- change **cameraFront** based on the **mouse input**
  - Changing the **direction vector** based on **mouse rotations** is a *little complicated* and requires **some trigonometry**

### Euler Angles

- Euler angles are **3 values**
  - Represent $\to$ **any rotation** in **3D** defined by **Leonhard Euler** in 1700s

- They are
  1. Pitch
  2. Yaw
  3. Roll

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210726002651752.png)

- The **pitch** $\to$ angle $\to$ how much we **look up / down** 
- Second image **yaw** $\to$ represents the **magnitude** of **left /right** 
- **roll** $\to$ represents how much **roll** of the **object**

---

- Each angle represented by **single value** with the **combination** of the 3 $\to$ calculate **any rotation vector** in **3D**

---

- Camera $\to$​ only care about **yaw / pitch values** therefore dont **discuss roll value here.**
  - Given some **pitch/yaw** value $\to$ convert to **3D vector** that represents the **new direction vector** 
    - Conversion of **yaw / pitch values** to a **direction vector** requires **trig**.

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210726003428110.png)

- If we define the hypotenuse to be of length `1` we know from trigonometry (soh cah toa) that the adjacent side's length is cos x/h=cos x/1=cos xcos⁡ x/h=cos⁡ x/1=cos⁡ x and that the opposing side's length is sin y/h=sin y/1=sin ysin⁡ y/h=sin⁡ y/1=sin⁡ y. 
- This gives us some general formulas for retrieving the length in both the `x` and `y` sides on right triangles, depending on the given angle. Let's use this to calculate the components of the direction vector.

- Imagine this triangle , looking from **top perspective**
  - With the **adjacent** / **opposite sides** being **parallel** to scene **x / z** axis
    - As if looking **down the y axis**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210726004600972.png)

- Visualize **yaw angle** to be **counter-clockwise** angle starting from **x side**
  - See that the **length** of the **x** side relates to $cos(\text{yaw})$ 
    - Similarly $\to$ how the **length** of the **z sides** relates to the $sin(yaw)$ 
- Take this with a given **yaw value** $\to$​ create a **camera direction vector** 

```c++
glm::vec3 direction;
direction.x = cos(glm::radians(yaw)); // note that we convert the angle to radians first.
direction.z = sin(glm::radians(yaw));
```

- Solves how we obtain **3D direction vector** from a **yaw value**
  - Pitch must be **included** as well
    - Look at the **y axis** side $\to$​ if we se sit on **xz plane** 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210726005321461.png)

- From this triangle $\to$​ see that the **direction's** y component equals **sin(pitch)**
  - Fill this in:

```c++
direction.y = sin(glm::radians(pitch));  
```

- From the **pitch value** $\to$​ see that the **xz** sides are **influenced** by $cos(pitch)$
  - So we must make sure **this** is part of the **direction vector**
    - With this **included** $\to$ obtain the **final direction** vector translated from **yaw / pitch euler angles**

---

```c++

direction.x = cos(glm::radians(yaw)) * cos(glm::radians(pitch));
direction.y = sin(glm::radians(pitch));
direction.z = sin(glm::radians(yaw)) * cos(glm::radians(pitch));
```

- Gives us **formulae** to convert **yaw / pitch** values to a **3D direction vector**
  - That we use for **looking around**

---

- Set the scene so everything is **positioned** in direction of the **negative z axis**
  - Looking at the **x / z** yaw triangle $\to$​ see that $\theta$ of **0** results in the **camera direction** vector
    - To **point towards** the positive **x axis** 
- To make the **camera points** towards  the **negative z axis** by default
  - Provide the **yaw** some default value of **90 degree clockwise** rotation
    - Positive degrees **rotate** *counter clockwise* so set the **default** *yaw* to

```c++
yaw = -90.0f;
```

## Mouse input

- Tell GLFW it should **hide the cursor** and *capture it*
- Idea is storing the **last frames** mouse values and in the **current frame** how much **they change**
  - The **higher** the **horizontal / vertical difference**
    - The more we **update** the **pitch  / yaw value** and therefore how much the **camera should move**,



- Capturing a **cursor** means that once the app has focus
  - The **mouse cursor** stays in the **center** of the **window** 
    - Unless the app loses focus or quits.
      - Can do this with one **configuration call**

```c++
glfwSetInputMode(window, GLFW_CURSOR, GLFW_CURSOR_DISABLED);  
```

- Mouse wont leave the window when moving it about, perfect for an **FPS system**

- Calculate pitch / yaw values $\to$ listen to **GLFW** mouse movement events
  - do this via creation of **callback function** of this **prototype**

```c++
void mouse_callback(GLFWwindow* window, double xpos, double ypos);
```

- **xpos** / **ypos**
  - Represent **current mouse coordinates** 
- As soon as we **register** the **callback function** with *GLFW* each time the **mouse moves**
  - The `mouse_callback` function is called:

```c++
glfwSetCursorPosCallback(window, mouse_callback);  
```

- Handling **mouse input** for a *fly style camera* 
  - There are various steps:
    1. calculate the **mouse offset** since the **last frame**
    2. **Add** the **offset values** to the camera **yaw / pitch**
    3. Add **constraints** to the **min / max** pitch values
    4. Calculate the **direction vector**
- Calculate offset:
  - store the last mouse positions in the app, which is init to the **center** of the **screen**

```c++
float lastX = 400, lastY = 300;
```

- In mouse **callback function**
  - calculate the **offset** movement between **last / current frame**

```c++

float xoffset = xpos - lastX;
float yoffset = lastY - ypos; // reversed since y-coordinates range from bottom to top
lastX = xpos;
lastY = ypos;

const float sensitivity = 0.1f;
xoffset *= sensitivity;
yoffset *= sensitivity;
```

- **Multiply** offset by a **sensitivity** value
  - If we **omit** this **multiplication** $\to$ the *mouse movement* would be **too strong** 
    - Fiddle with this value as you like.
- Add **offset values** to the **globally** declared **pitch / yaw values.**

```c++
yaw   += xoffset;
pitch += yoffset; 
```

- Add **constraints** to prevent **odd camera movements**
  - Causes a **LookAt** flip once the **direction vector** is *parallel* to the **world up direction** 
    - Pitch must be **constrained** in a way that users cant look higher than **89 degrees** (90 = look at flip)
- Also not below -89 

```c++

if(pitch > 89.0f)
  pitch =  89.0f;
if(pitch < -89.0f)
  pitch = -89.0f;
```

- No constraint required on the **yaw.**

- Final step calculate the **direction vector** using formulae of **last section**

```c++
glm::vec3 direction;
direction.x = cos(glm::radians(yaw)) * cos(glm::radians(pitch));
direction.y = sin(glm::radians(pitch));
direction.z = sin(glm::radians(yaw)) * cos(glm::radians(pitch));
cameraFront = glm::normalize(direction);
```

- Direction  vector on this contains **all the rotations** calculated from the **mouse movement**
  - `cameraFront` vector is **included** in glm **lookAt** function $\to$ we can go

- Add values to set initial mouse focus inputs to prevent large offsets.

```c++
if (firstMouse) // initially set to true
{
    lastX = xpos;
    lastY = ypos;
    firstMouse = false;
}
```

- Final code becomes

```c++
void mouse_callback(GLFWwindow* window, double xpos, double ypos)
{
    if (firstMouse)
    {
        lastX = xpos;
        lastY = ypos;
        firstMouse = false;
    }
  
    float xoffset = xpos - lastX;
    float yoffset = lastY - ypos; 
    lastX = xpos;
    lastY = ypos;

    float sensitivity = 0.1f;
    xoffset *= sensitivity;
    yoffset *= sensitivity;

    yaw   += xoffset;
    pitch += yoffset;

    if(pitch > 89.0f)
        pitch = 89.0f;
    if(pitch < -89.0f)
        pitch = -89.0f;

    glm::vec3 direction;
    direction.x = cos(glm::radians(yaw)) * cos(glm::radians(pitch));
    direction.y = sin(glm::radians(pitch));
    direction.z = sin(glm::radians(yaw)) * cos(glm::radians(pitch));
    cameraFront = glm::normalize(direction);
}  
```

## Zoom

- A **zooming interface** 
  - For previous chapter $\to$ said the **FOV** defines how we see the scene
- When **fov** becomes **smaller**
  - The scenes **projected space** becomes **smaller**
    - This smaller space $\to$ projected over **same NDC**
      - Gives the **illusion** of **zooming**

```c++
void scroll_callback(GLFWwindow* window, double xoffset, double yoffset)
{
    fov -= (float)yoffset;
    if (fov < 1.0f)
        fov = 1.0f;
    if (fov > 45.0f)
        fov = 45.0f; 
}
```

- y offset values tells us the **ammount** we scrolled vertically
  - When this **callback** is called
    - Can **change** the **content** of the **globally declared** *fov variable* 
      - As **45 degrees** is default fov
        - Constrain between 1 and 45.

- Alter **our perspective projection matrix now**

```c++
projection = glm::perspective(glm::radians(fov), 800.0f / 600.0f, 0.1f, 100.0f);  
```

- Register scroll **callback function**

---

```c++
glfwSetScrollCallback(window, scroll_callback); 
```

