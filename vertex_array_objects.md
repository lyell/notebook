# Vertex Array Objects

## List of Steps

1. Setup
	1. Create a VAO
	2. Bind the VAO
	2. Create the Buffers
	3. Bind the Buffers
	4. Send the Buffer data
	5. Enable Shader attributes
2. Drawing

## Setup
 
### Create a VAO

Create a Vertex Array Object (VAO). Do this once during initialization.

```
GLuint vertexArray;
glGenVertexArrays(1, &vertexArray);
glBindVertexArray(vertexArray);
```

### Setup Buffers

Create the Buffer objects and store the buffer data. Do this once during initialization.

```
// C-array containing vertex coords
float bufferData[];

GLuint vertexBuffer;
glGenBuffers(1, &vertexBuffer);
glBindBuffer(GL_ARRAY_BUFFER, vertexBuffer);
glBufferData(GL_ARRAY_BUFFER, sizeof(bufferData), bufferData, GL_STATIC_DRAW);
glBindBuffer(GL_ARRAY_BUFFER, 0);

// Do the same for other buffers, or combine all buffers into one
```

### Bind to Shader attributes

Bind all the attrib variables for the shader and store in the VAO

```
GLuint programId;

// Use the shader program
glUseProgram(programId)

// get the shader attrib index for each buffer
// e.g. get the location of the 'position' input
GLint positionLocation = glGetAttribLocation(programId, 'position')

// Store the vertex attrib info in the VAO
glEnableVertexAttribArray(positionLocation);
glBindBuffer(GL_ARRAY_BUFFER, vertexBuffer);
glVertexAttribPointer(
	positionLocation,   // Location of vertex attribute
	3,                  // size of vertex
	GL_FLOAT,           // type
	GL_FALSE,           // normalized?
	0,                  // stride
	(void*)0            // offset
);

// Unbind the buffer
glBindBuffer(GL_ARRAY_BUFFER, 0);
```

### Unbind the VAO

Unbind the vertex array object

```
glBindVertexArray(0);
```

### Drawing

During the draw loop, bind the VAO and draw the elements

```
glUseProgram(programId);
glBindVertexArray(vertexArray);
glDrawArrays(
	GL_TRIANGLE_STRIP,   // mode
	start,               // starting index in bound arrays
	count                // count of indices to draw
);