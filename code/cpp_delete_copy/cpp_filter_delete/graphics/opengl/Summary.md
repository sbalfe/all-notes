# OpenGL

- Not a library > just a specification.
  - This function exists > take x,y,z params and return this value etc.

- No actual download
  - Code > by GPU manufacturer > the nvidia drivers are what performs this.
    - Not the same implementation on each GPU AMD vs NVIDIA.
- Its **not open source** as the **code cant be seen** (driver code)
  - Some may be visible however but not actual implementation low level not ever seen.

- One of the easiest libraries to run
- Shader
  - Its code that runs on the GPU > allow us to run CPU code on the GPU

---

## glfw

- Basic setup for creating the windows.

### Linking

- Add `dependencies` folder
  - Within add **glfw** 
    - In here add the glfw binaries of `visual studio 2019` and `include` 
- Go to project properties and `c/c++` , add additional include directories to point to `$(SolutionDir)\dependecies\glfw\include`
  - Then you can use `#include<GLFW/glfw.h>`

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210614115517477.png)

- Then add the to `Linker` > `General` 

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210614115618024.png)

- Then add to `input` > additional dependencies
  - The files that are required by windows which you can look at the docs for example in here it tells you the specified lib if you have link error
  - https://docs.microsoft.com/en-us/windows/win32/api/shellapi/nf-shellapi-dragqueryfilea

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210614115726752.png)

- glfw.lib is in the binary ofc the others are built into windows.
  - Alternatively do  %(AdditionalDependencies) to include these automatically.

### Contexts

An **OpenGL context** represents many things. A context stores all of the state associated with this instance of OpenGL. It represents the (potentially visible) [default framebuffer](https://www.khronos.org/opengl/wiki/Default_Framebuffer) that rendering commands will draw to when not drawing to a [framebuffer object](https://www.khronos.org/opengl/wiki/Framebuffer_Object). Think of a context as an object that holds all of OpenGL; when a context is destroyed, OpenGL is destroyed.

Contexts are localized within a particular process of execution (an application, more or less) on an operating system. A process can create multiple OpenGL contexts. Each context can represent a separate viewable surface, like a window in an application.

Each context has its own set of [OpenGL Objects](https://www.khronos.org/opengl/wiki/OpenGL_Object), which are independent of those from other contexts. A context's objects can be shared with other contexts. Most OpenGL objects are sharable, including [Sync Objects](https://www.khronos.org/opengl/wiki/Sync_Object) and [GLSL Objects](https://www.khronos.org/opengl/wiki/GLSL_Object). [Container Objects](https://www.khronos.org/opengl/wiki/Container_Object) are not sharable, nor are [Query Objects](https://www.khronos.org/opengl/wiki/Query_Object).

Any object sharing must be made explicitly, either as the context is created or before a newly created context creates any objects. However, contexts do not *have* to share objects; they can remain completely separate from one another.

In order for any OpenGL commands to work, a context must be *current*; all OpenGL commands affect the state of whichever context is current. The current context is a thread-local variable, so a single process can have several threads, each of which has its own current context. However, a single context cannot be current in multiple threads at the same time.

---

## Modern OpenGL

- Access driver dll files
  - Library used to help access this for you.