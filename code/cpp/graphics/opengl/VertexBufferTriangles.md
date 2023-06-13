# Vertex Buffers and Triangles

- Ignore vertex
  - A memory buffer > array of memory bytes which we can add to.
  - This is a buffer of memory present in our GPU itself (vram).

- draw call > blob of data > read and draw to the screen.
  - Tell the cpu written code to GPU > draw a triangle and tell it how to do to this.

- Select this buffer / shader > then draw a triangle
  - Depending on whether the buffer /shader is specified decides the triangle drawn.

https://docs.gl/ - find information on stuff

![image](https://github.com/sbalfe/all-notes/blob/master/images/image-20210614132159132.png)

#### Stride

- The ammount of bytes between each vertex.
  - 32 bytes into buffer > start of second buffer etc.

https://stackoverflow.com/questions/22296510/what-does-stride-mean-in-opengles/22297383

#### Pointer

- Pointer into some attribute
  - In the space of the vertex. (position / texture coordinate/ normal)
  - offset of position  = 0 , first byte
  - 12 bytes forward > begin of texture coordinate = 12  bytes.

https://cognitivewaves.wordpress.com/opengl-terminology-demystified/

https://paroj.github.io/gltut/Basics/Tut01%20Following%20the%20Data.html - useful af read this shit.

```c++
   /*
    * x and y coordinates of each indice in the screen.
    * 
    * vertex = point in the geometry, not just a simple position > can contain much more data (attributes, position is an attribute , texture attribute etc.)
    */
    float positions[6] = {
       -0.5f,-0.5f,
        0.0f,0.5f,
        0.5f,-0.5f
    };

    unsigned int buffer;
    // this unsigned int is the id of the generated buffer
    /*
        open gl given certain values as its a state machine > id of the object to use vertex buffer / shader etc.
        use the number when to use said object. 
    */
    glGenBuffers(1, &buffer);

    // bind the current buffer that was just created.
    //https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/glBindBuffer.xhtml
    glBindBuffer(GL_ARRAY_BUFFER, buffer); // this is what is selected currently as the current bind.

    glEnableVertexAttribArray(0);

    //https://docs.gl/gl4/glVertexAttribPointer
    //https://stackoverflow.com/questions/24876647/understanding-glvertexattribpointer
    glVertexAttribPointer(0, 2, GL_FLOAT, GL_FALSE, 2 * sizeof(float), 0);

    //https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/glBufferData.xhtml
    glBufferData(GL_ARRAY_BUFFER, 6 * sizeof(float), positions, GL_STATIC_DRAW);
```

**glGenBuffers** - https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/glGenBuffers.xhtml

**glBindBuffer** - https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/glBindBuffer.xhtml

**glEnableVertexAttribArray** - https://www.khronos.org/registry/OpenGL-Refpages/gl2.1/xhtml/glEnableVertexAttribArray.xml

**glVertexAttribPointer** - https://docs.gl/gl4/glVertexAttribPointer

https://en.wikibooks.org/wiki/OpenGL_Programming/Modern_OpenGL_Tutorial_03 - explain attributes

