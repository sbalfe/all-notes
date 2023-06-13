# Transformations

- Much **less costly** method than **changing the vertices** and *reconfiguring buffers* to **move the objects about**.
  - Transform an object via using **multiple matrix objects**.

---

## Scaling

- Scaling a vector
  - Increasing the **length** of the **arrow** by a specific ammount
- Working in **2/3D** , define scaling by a **vector** of **2 / 3** *scaling variables*
  - Each **scaling one axis** (*x , y , z*)
- Try scaling **vector** $v = (3,2)$
  - Scale the **vector** along the **x axis** by **0.5**
    - Making it **twice as narrow**
  - Scale the **vector** by **2** along the **y axis** making it **twice as high**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210620165425424.png)

- OpenGL operates in **3D** space for this **2D** case just set **z axis** to **1**, thus leaving it **unharmed**
  - The **scaling operation** was *performed* in a **non uniform scale**
    - As the **scaling factor** is not **the same on each axis**.
      - If the **scalar** would be **equal** on **all axes** it would be **uniform scale.**

## Translation

- Just **adding another vector** on top of the **original vector**
  - Returns a **new vector** with a **different position**, thus *moving the vector* based on the **translation vector**

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210620170151160.png)

- Works as **all of the translation** values are **multiplied** by the **vectors** *w column* and **added** to the **original** values
  - Remembering the **matrix multiplication values**.
    - This is **not possible** with some **3 x 3 matrix**.

#### Homogenous coordinates.

**Homogeneous coordinates**

- The `w` component of a vector is also known as a homogeneous coordinate. To get the 3D vector from a homogeneous vector we divide the `x`, `y` and `z` coordinate by its `w` coordinate. We usually do not notice this since the `w` component is `1.0` most of the time. Using homogeneous coordinates has several advantages: it allows us to do matrix translations on 3D vectors (without a `w` component we can't translate vectors) and in the next chapter we'll use the `w` value to create 3D perspective.

- Also, whenever the homogeneous coordinate is equal to `0`, the vector is specifically known as a direction vector since a vector with a `w` coordinate of `0` cannot be translated.

```c++
/ define a vector with 4 items, homogenous coordinate set to 1.0f
    glm::vec4 vec(1.0f, 0.0f, 0.0f, 1.0f);
    // create an identity matrix with 1 on all the diagonals, no init results in null matrix
    glm::mat4 trans = glm::mat4(1.0f);
    /* 
        create transformation matrix by passing the identity matrix to glm::translate

        together with the translation vector , given matrix is multiplied with a translation matrix, returning the resulting matrix

        multiply our vector by the transformation matrix and then output the result
    */
    trans = glm::translate(trans, glm::vec3(1.0f, 1.0f, 0.0f));
    vec = trans * vec;
    std::cout << vec.x << vec.y << vec.z << std::endl;
```

```c++
    /*
        scale and rotate the container object from the previous chapter
    */

    /*
        create 4 x 4 identity matrix
    */
    glm::mat4 trans = glm::mat4(1.0f);

    /*
        rotate 90 degrees around the z axis.

        expects the angle in radians therefore convert degrees
    */
    trans = glm::rotate(trans, glm::radians(90.0f), glm::vec3(0.0, 0.0, 1.0));

    /*
        scale the container by 0.5 on each axis
    */
    trans = glm::scale(trans, glm::vec3(0.5, 0.5, 0.5));
```

```c++
#version 330 core
layout (location = 0) in vec3 aPos;
layout (location = 1) in vec2 aTexCoord;

out vec2 TexCoord;
  
uniform mat4 transform;

void main()
{
    gl_Position = transform * vec4(aPos, 1.0f);
    TexCoord = vec2(aTexCoord.x, aTexCoord.y);
} 
```

> GLSL also has `mat2` and `mat3` types that allow for swizzling-like operations just like vectors. All the aforementioned math operations (like scalar-matrix multiplication, matrix-vector multiplication and matrix-matrix multiplication) are allowed on the matrix types. Wherever special matrix operations are used we'll be sure to explain what's happening.

- GLSL has a **mat4 type**
  - We adapt the **vertex shader** to accept a **mat4 uniform variable**.
    - Then multiply the **position vector** by the **matrix uniform**.

- We added the **uniform** and **multiplied** the **position vector**
  - With the **transformation** matrix before **passing** it to **gl_Position**.
    - Container should **be twice as small** and *rotated 90 degrees*
- Pass the **transformation matrix** to the **shader though**

```c++
/*
	query the location of the uniform variable , then send the matrix data to the shaders 
	
	using glUniform with Matrix4v as its postfix.
	
*/
unsigned int transformLoc = glGetUniformLocation(ourShader.ID, "transform");

/*
	arg 1 the location of the uniforms location
	
	arg 2 tell OpenGL how many matrices we'd like to send > which is 1
	
	arg 3 asks if we want to transpose the matrix, which is swapping the rows and columns
	
	OpenGL often use an internal matrix,  called column-major ordering, the default matrix layout in GLM, therefore no need 	to transpose the matrices
	
	final arg > the actual matrix data, GLM stores the matrices data in a form that does not match OpenGL expectations
	therefore convert using value_ptr
*/
glUniformMatrix4fv(transformLoc, 1, GL_FALSE, glm::value_ptr(trans));
```

```c++
// first create our identity matrix to build the transformation matrix
glm::mat4 trans = glm::mat4(1.0f);

// translate 1/2 units x direction and -1/2 y direction down
trans = glm::translate(trans, glm::vec3(0.5f, -0.5f, 0.0f));
// apply a rotation based on the time around the z axis.
trans = glm::rotate(trans, (float)glfwGetTime(), glm::vec3(0.0f, 0.0f, 1.0f));


```

