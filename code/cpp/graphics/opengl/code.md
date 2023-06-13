```c++

#include <Gl/glew.h>
#include <GLFW/glfw3.h>
#include <iostream>
#include <fstream>
#include <string>
#include <sstream>

// asssertion function to call breakpoint on specified variable being false
#define ASSERT(x) if (!(x)) __debugbreak();

// checks for errors on every call and returns the file / line number it occured with the error number also
#define GLCall(x) GLClearError();\
    x;\
    ASSERT(GLLogCall(#x, __FILE__, __LINE__))
    

struct ShaderProgramSource{
    std::string VertexSource;
    std::string FragmentSource;
};


static void GLClearError() {
    
    while (glGetError() != GL_NO_ERROR);
            
}

static bool GLLogCall(const char* function, const char* file, int line) {
    // obtain all the errors and output them
    while (GLenum error = glGetError()) {
        std::cout << "[OpenGL Error]  ( " << error << " )" << function << " " << file << " : " << line << std::endl;
        return false;
    }
    return true;
}

static ShaderProgramSource ParseShader(const std::string& filepath) {


    
    std::ifstream stream(filepath);
   
    enum class ShaderType {

        NONE = -1, VERTEX = 0, FRAGMENT = 1
    };
    
    ShaderType type = ShaderType::NONE;

    std::string line;
    std::stringstream ss[2];
    while (getline(stream, line)) {
        // check whether it found this otherwise we are reading in the lines as code
        // switch the type around to decide which index in ss to read in the code to.
        if (line.find("#shader") != std::string::npos) {

            if (line.find("vertex") != std::string::npos) {
                //set mode to vertex
                type = ShaderType::VERTEX;
            }
            else if (line.find("fragment") != std::string::npos) {
                // set mode to fragment
                type = ShaderType::FRAGMENT;
            }

        }
        else {
            ss[(int)type] << line << '\n';
        }
    }


    // return the defined struct without using name but just using {} and the values to infer automatically what it is.
    return { ss[0].str(), ss[1].str() };
}

static unsigned int CompileShader(unsigned int type, const std::string& source) {

    unsigned int id = glCreateShader(type);
    const char* src = source.c_str(); // pointer to a null terminate sequence of character representing current value of string object
    glShaderSource(id, 1, &src, nullptr);
    glCompileShader(id);

    int result;
    glGetShaderiv(id, GL_COMPILE_STATUS, &result);

    // error occured in the shader, information used to debug
    if (result == GL_FALSE) {
        int length;
        glGetShaderiv(id, GL_INFO_LOG_LENGTH, &length);
        char* message = (char *)  _malloca(length *sizeof(char)); // dynamic allocation on the stack, frees automatically when out of scope
        glGetShaderInfoLog(id, length, &length, message);
        std::cout << "failed to compile shader " << (type == GL_VERTEX_SHADER ? "vertex" : "fragment") <<std::endl << message << std::endl;
        glDeleteShader(id);
        return 0;
    }

    //TODO : Error handling 

    return id;
}


static unsigned int CreateShader(const std::string& vertexShader, const std::string& fragmentShader) {

    unsigned int program =  glCreateProgram();
    unsigned int vs = CompileShader(GL_VERTEX_SHADER, vertexShader);
    unsigned int fs = CompileShader(GL_FRAGMENT_SHADER, fragmentShader);

    glAttachShader(program, vs);
    glAttachShader(program, fs); 
    glLinkProgram(program);
    glValidateProgram(program);


    // remove after the program has compiled. 
    glDeleteShader(vs);
    glDeleteShader(fs);

    return program;
}

int main(void)
{
    GLFWwindow* window;

    /* Initialize the library */
    if (!glfwInit())
        return -1;

    

    /* Create a windowed mode window and its OpenGL context */
    window = glfwCreateWindow(640, 480, "Hello World", NULL, NULL);
    if (!window)
    {
        glfwTerminate();
        return -1;
    }

    /* Make the window's context current , this is where we make the opengl context which makes glewinit now work after it has this*/
    glfwMakeContextCurrent(window);

    if (glewInit() != GLEW_OK)
        std::cout << "Error!" << std::endl;


    std::cout << glGetString(GL_VERSION) << std::endl;

    /*
    * x and y coordinates of each indice in the screen.
    * vertex = point in the geometry, not just a simple position > can contain much more data (attributes, position is an attribute , texture attribute etc.)
    */

    float positions[] = {
       -0.5f,-0.5f,
        0.5f,-0.5f,
        0.5f, 0.5f,
        -0.5f, 0.5f,
    };

    // index buffer using the correct indices to access from positions without repetitiuon
    unsigned int indices [] {

        0, 1, 2,2,3,0

    };

    // this unsigned int is the id of the generated buffer
    unsigned int buffer;

    /*
        open gl given certain values as its a state machine > id of the object to use vertex buffer / shader etc.
        use the number when to use said object. 
    */

    /*
        Generate 1 buffer allocated in location of the unsigned int specified    
    */
    GLCall(glGenBuffers(1, &buffer));

    // bind the current buffer that was just created.
    //https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/glBindBuffer.xhtml
    //https://stackoverflow.com/questions/14802854/what-does-the-gl-array-buffer-target-mean-in-glbindbuffer -gl ARRAY buffer explained
    GLCall(glBindBuffer(GL_ARRAY_BUFFER, buffer)); // this is what is selected currently as the current bind.

    //https://www.khronos.org/registry/OpenGL-Refpages/gl4/html/glBufferData.xhtml
    GLCall(glBufferData(GL_ARRAY_BUFFER, 6 * 2 * sizeof(float), positions, GL_STATIC_DRAW));

    // Enable 
    GLCall(glEnableVertexAttribArray(0));

    //https://docs.gl/gl4/glVertexAttribPointer
    //https://stackoverflow.com/questions/24876647/understanding-glvertexattribpointer
    GLCall(glVertexAttribPointer(0, 2, GL_FLOAT, GL_FALSE, 2 * sizeof(float), 0));

    // index buffer object
    unsigned int ibo;
    GLCall(glGenBuffers(1, &ibo));
    GLCall(glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, ibo));
    GLCall(glBufferData(GL_ELEMENT_ARRAY_BUFFER, 6  * sizeof(unsigned int), indices, GL_STATIC_DRAW));

  
    std::cout << "test" << std::endl;
    ShaderProgramSource source = ParseShader("res/shaders/Basic.shader");
    unsigned int shader = CreateShader(source.VertexSource, source.FragmentSource);
    glUseProgram(shader);


    /* Loop until the user closes the window */
    while (!glfwWindowShouldClose(window))
    {
        /* Render here */

        /* clear buffer to preset values, param passed in indicates buffer enable for color writing*/
        glClear(GL_COLOR_BUFFER_BIT);

        // what is required finally to draw triange. https://docs.gl/gl4/glDrawArrays
        //glDrawArrays(GL_TRIANGLES, 0, 6);
        // must be an unsigned integer.

        GLClearError();

        GLCall(glDrawElements(GL_TRIANGLES, 6, GL_INT, nullptr));

      

        /* Swap front and back buffers https://www.glfw.org/docs/3.0/group__context.html#gac84759b1f6c2d271a4fea8ae89ec980d */
        glfwSwapBuffers(window);

        /* Poll for and process events https://www.glfw.org/docs/3.3/group__window.html#ga37bd57223967b4211d60ca1a0bf3c832 */
        glfwPollEvents();
    }

    //glDeleteProgram(shader);
    glfwTerminate();
    return 0;
}
```

```c++

#include <Gl/glew.h>
#include <GLFW/glfw3.h>
#include <iostream>
#include <fstream>
#include <string>
#include <sstream>

#define ASSERT(x) if (!(x)) __debugbreak();


#define GLCall(x) GLClearError();\
    x;\
    ASSERT(GLLogCall(#x, __FILE__, __LINE__))
    

struct ShaderProgramSource{
    std::string VertexSource;
    std::string FragmentSource;
};


static void GLClearError() {
    
    while (glGetError() != GL_NO_ERROR);
            
}

static bool GLLogCall(const char* function, const char* file, int line) {
    while (GLenum error = glGetError()) {
        std::cout << "[OpenGL Error]  ( " << error << " )" << function << " " << file << " : " << line << std::endl;
        return false;
    }
    return true;
}

static ShaderProgramSource ParseShader(const std::string& filepath) {

    std::ifstream stream(filepath);
    enum class ShaderType {

        NONE = -1, VERTEX = 0, FRAGMENT = 1
    };
    ShaderType type = ShaderType::NONE;
    std::string line;
    std::stringstream ss[2];
    while (getline(stream, line)) {
        if (line.find("#shader") != std::string::npos) {

            if (line.find("vertex") != std::string::npos) {
               
                type = ShaderType::VERTEX;
            }
            else if (line.find("fragment") != std::string::npos) {
               
                type = ShaderType::FRAGMENT;
            }
        }
        else {
            ss[(int)type] << line << '\n';
        }
    }
    return { ss[0].str(), ss[1].str() };
}

static unsigned int CompileShader(unsigned int type, const std::string& source) {

    unsigned int id = glCreateShader(type);
    const char* src = source.c_str(); 
    glShaderSource(id, 1, &src, nullptr);
    glCompileShader(id);
    int result;
    glGetShaderiv(id, GL_COMPILE_STATUS, &result);  
    if (result == GL_FALSE) {
        int length;
        glGetShaderiv(id, GL_INFO_LOG_LENGTH, &length);
        char* message = (char *)  _malloca(length *sizeof(char)); 
        glGetShaderInfoLog(id, length, &length, message);
        std::cout << "failed to compile shader " << (type == GL_VERTEX_SHADER ? "vertex" : "fragment") <<std::endl << message << std::endl;
        glDeleteShader(id);
        return 0;
    }
    return id;
}


static unsigned int CreateShader(const std::string& vertexShader, const std::string& fragmentShader) {
    unsigned int program =  glCreateProgram();
    unsigned int vs = CompileShader(GL_VERTEX_SHADER, vertexShader);
    unsigned int fs = CompileShader(GL_FRAGMENT_SHADER, fragmentShader);
    glAttachShader(program, vs);
    glAttachShader(program, fs); 
    glLinkProgram(program);
    glValidateProgram(program);
    glDeleteShader(vs);
    glDeleteShader(fs);

    return program;
}

int main(void)
{
    GLFWwindow* window;

    if (!glfwInit())
        return -1;

    glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 4.6);
    glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR,4.6 );
    glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
    

    window = glfwCreateWindow(640, 480, "Hello World", NULL, NULL);
    if (!window)
    {
        glfwTerminate();
        return -1;
    }
    glfwMakeContextCurrent(window);
    glfwSwapInterval(1);
    if (glewInit() != GLEW_OK)
        std::cout << "Error!" << std::endl;

    std::cout << glGetString(GL_VERSION) << std::endl;
    float positions[] = {
       -0.5f,-0.5f,
        0.5f,-0.5f,
        0.5f, 0.5f,
        -0.5f, 0.5f,
    };

   
    unsigned int indices [] {

        0, 1, 2,2,3,0

    };

    unsigned int vao;

    GLCall(glGenVertexArrays(1, &vao));
    GLCall(glBindVertexArray(vao))

    //Vertex buffer for storing the vertices
    unsigned int buffer;
    GLCall(glGenBuffers(1, &buffer));
    GLCall(glBindBuffer(GL_ARRAY_BUFFER, buffer)); 
    GLCall(glBufferData(GL_ARRAY_BUFFER, 4 * 2 * sizeof(float), positions, GL_STATIC_DRAW));

    GLCall(glEnableVertexAttribArray(0));

    // specify the layout of the vertex buffer
    GLCall(glVertexAttribPointer(0, 2, GL_FLOAT, GL_FALSE, 2 * sizeof(float), 0));

    unsigned int ibo;
    GLCall(glGenBuffers(1, &ibo));
    GLCall(glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, ibo));
    GLCall(glBufferData(GL_ELEMENT_ARRAY_BUFFER, 6  * sizeof(unsigned int), indices, GL_STATIC_DRAW));

    ShaderProgramSource source = ParseShader("res/shaders/Basic.shader");
    unsigned int shader = CreateShader(source.VertexSource, source.FragmentSource);
    glUseProgram(shader);

    float r = 0.0f;
    float increment = 0.05f;

    // locate the data of the uniform variable
    GLCall(int location = glGetUniformLocation(shader, "u_Color"));
    ASSERT(location != -1);

    // set the data here
    GLCall(glUniform4f(location, 0.2f, 0.3f, 0.8f, 1.0f));


    GLCall(glUseProgram(0));
    GLCall(glBindBuffer(GL_ARRAY_BUFFER, 0));
    GLCall(glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, 0));
   

    while (!glfwWindowShouldClose(window)){
        glClear(GL_COLOR_BUFFER_BIT);
        //glDrawArrays(GL_TRIANGLES, 0, 6);
    

        GLCall(glUseProgram(shader))
        GLCall(glUniform4f(location, r, 0.3f, 0.8f, 1.0f));


        GLCall(glBindVertexArray(vao));

        GLCall(glBindBuffer(GL_ARRAY_BUFFER, buffer));

        GLCall(glEnableVertexAttribArray(0));
        GLCall(glVertexAttribPointer(0, 2, GL_FLOAT, GL_FALSE, sizeof(float) * 2, 0));


        GLCall(glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, ibo));

        GLCall(glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, nullptr));

        if (r > 1.0f) 
        {
            increment = -0.5f;
        }
        else if (r < 0.0f) {
            increment = 0.05f;
        }

        r += increment;

        glfwSwapBuffers(window);
        glfwPollEvents();
    }
    //glDeleteProgram(shader);
    glfwTerminate();
    return 0;
}
```

