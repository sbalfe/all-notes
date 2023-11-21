# Creating a window

- First create an **OpenGL** and **an application** window to **draw in**.
  - These are **specific per operating system** and **OpenGL** *purposefully* tries **to abstract** itself from **these operations**.
- Must **create a window** / define a **context** / handle **user input** all **by ourselves**
  - Use GLFW

## GLFW

- GLFW is a library, written in C, specifically targeted at OpenGL. GLFW gives us the **bare necessities** required for rendering goodies to the screen.
-  It allows us to create an **OpenGL context**, define **window parameters**, and **handle user input,** which is plenty enough for our purposes.

- The focus of this and the next chapter is to get **GLFW up and running**, making sure it properly **creates an OpenGL context** and that it displays a simple window for us to mess around in. This chapter takes a step-by-step approach in retrieving, building and linking the GLFW library. 
- We'll use Microsoft Visual Studio 2019 IDE as of this writing (note that the process is the same on the more recent visual studio versions). If you're not using Visual Studio (or an older version) don't worry, the process will be similar on most other IDEs.

# Hello Window

```c++
int main()
{
    glfwInit();
    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
    //glfwWindowHint(GLFW_OPENGL_FORWARD_COMPAT, GL_TRUE);
  
    return 0;
}
```

- List of all possible options to pass in here:
  - http://www.glfw.org/docs/latest/window.html#window_hints

- Focus of this book is OpenGL version 3.3 $\to$ we would like to tell  GLFW that 3.3 is the OpenGL version we want to use,
  - Creates correct arrangements   
    -  When the user does not have proper OpenGL version GLFW fails to run.
      - Set **major** and **minor** version both to 3
- Also tell GLFW we wish to **access** to **core-profile** 
  - Telling GLFW we want to use **core profile** means we will get **access** to a **smaller subset** of OpenGL features with **no backwards compatible features we longer need**

```c++
GLFWwindow* window = glfwCreateWindow(800, 600, "LearnOpenGL", NULL, NULL);
if (window == NULL)
{
    std::cout << "Failed to create GLFW window" << std::endl;
    glfwTerminate();
    return -1;
}
glfwMakeContextCurrent(window);
```

```c++

if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))
{
    std::cout << "Failed to initialize GLAD" << std::endl;
    return -1;
}    
```

```c++
glViewport(0, 0, 800, 600);
```

> Behind the scenes OpenGL uses the data specified via glViewport to transform the 2D coordinates it processed to coordinates on your screen. For example, a processed point of location `(-0.5,0.5)` would (as its final transformation) be mapped to `(200,450)` in screen coordinates. Note that processed coordinates in OpenGL are between -1 and 1 so we effectively map from the range (-1 to 1) to (0, 800) and (0, 600).

```c++
void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{
    glViewport(0, 0, width, height);
}  
```



- The moment said user **resizes the window** the *viewport* should be **adjusted** as well
  - Register a **callback** function on the **window** that is **called each time** the window **is resized**
    - The **resize callback** has following **prototype**.

```c++
while(!glfwWindowShouldClose(window))
{
    glfwSwapBuffers(window);
    glfwPollEvents();    
}
```

> **Double buffer**
>
> - When an application **draws in a single buffer** the resulting **image may display flickering issues**. This is because the **resulting output** image is **not drawn in an instant**, but **drawn pixel by pixel** and usually from **left to right and top to bottom**. 
> - Because this **image is not displayed** at an **instant to the user while still being rendered to**, the result may **contain artifacts**. To circumvent these issues, **windowing applications apply a double buffer for rendering**.
> -  The **front** buffer contains the **final output image** that is **shown at the screen**, while all the **rendering commands** draw to the **back** buffer.
> -  As soon as all the **rendering commands** are finished we **swap** the **back buffer to the front buffer** so the image can be **displayed without** still **being rendered to, removing all the aforementioned artifacts.**

```c++
glfwTerminate();
return 0;
```

## Input

- We want to have some **input control** in **GLFW** 
  - Achieve with several of **GLFW input function** 
    - We'll **by using GLFW** *glfwGetKey* $\to$ function **takes** the **window** as **input** together with a key
- The **function returns** whether this **key** is **currently being pressed**
  - We are **creating** a *processInput* function to **keep all input code organized**.

```c++
void processInput(GLFWwindow *window)
{
    if(glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)
        glfwSetWindowShouldClose(window, true);
}
```

- Check whether the **user has pressed the escape key**
  - If **not's pressed** $\to$ *glfwGetKey* $\to$ return **GLFW_RELEASE**
    - If the user **did press the escape key** $\to$ close **GLFW** by setting its **WindowShouldClose** property to **true** using **glfwSetWindowShouldClose**.

## Rendering

- Want to **place** all **the rendering commands** in the *render loop*.
  - We **want to execute** all the **rendering commands** every **iteration** or **frame of the loop**
    - This would look a **bit like this**.

```c++
// render loop
while(!glfwWindowShouldClose(window))
{
    // input
    processInput(window);

    // rendering commands here
    ...

    // check and call events and swap the buffers
    glfwPollEvents();
    glfwSwapBuffers(window);
}
```

```c++
glClearColor(0.2f, 0.3f, 0.3f, 1.0f);
glClear(GL_COLOR_BUFFER_BIT);
```

> As you may recall from the *OpenGL* chapter, the glClearColor function is a *state-setting* function and glClear is a *state-using* function in that it uses the current state to retrieve the clearing color from.

