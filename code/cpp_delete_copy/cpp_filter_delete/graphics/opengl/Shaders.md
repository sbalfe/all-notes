# Shaders

- Some GPU > provide default shader therefor the code works without the specified shader.
- Shader > a program that runs on the GPU.
- Text as string on computer > send to GPU.



## Vertex Shaders

- We have sent data to GPU and initiated a draw call
- draw call > call vertex shader > call fragment shader > pixels on screen

---

- Vertex shader called for every vertex we call
  - Called 3 times for each of the vertices on a triangle example.
  - Specify where the positions are at index 0.
- Decide where to pass arguments to next shader. 

## Fragment shader / Pixel shader

- Run once for every pixel that has to be ==rasterized== (drawn to screen) as the screen is a pixel array.
  - Decides the output colour for the dedicated pixel that is to be generated.
- Calls many times as its for filling in the entire shape.
  - Must do most operations from the vertex shader and pass to fragment shader.
- Some things must be done per pixel
  - Each one has colour value > lighting / environment / texture for example.

