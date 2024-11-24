# Wallpaper by Shader

## Is it possible to make shader app in py to generate wallpapers?

Yes, it is possible to create a shader-based application in Python to generate wallpapers. Python provides several libraries and frameworks that you can use to write shaders, manipulate graphics, and render images. Here's a high-level approach:

---

### 1. **Select a Graphics Library**

Python doesn't directly execute GPU shaders, but you can use libraries that support OpenGL or similar APIs:

-   **PyOpenGL**: Direct access to OpenGL shaders.
-   **Pygame**: Combined with OpenGL for shader-based rendering.
-   **ModernGL**: Simplifies working with OpenGL in Python.

---

### 2. **Write a Shader**

Shaders are written in GLSL (OpenGL Shading Language). You'll write vertex and fragment shaders for effects. For example:

-   **Vertex Shader**: Transforms vertices of a shape.
-   **Fragment Shader**: Colors each pixel, creating visual effects.

Example fragment shader for a gradient:

```glsl
#version 330 core
out vec4 FragColor;

void main() {
    vec2 uv = gl_FragCoord.xy / vec2(800, 600); // Normalize to screen size
    FragColor = vec4(uv.x, uv.y, 0.5, 1.0);    // Gradient color
}
```

---

### 3. **Render the Shader**

Use Python to create a rendering window and pass the shader code to the GPU. With **PyOpenGL** or **ModernGL**, you can create a framebuffer and render the shader output.

---

### 4. **Save as Wallpaper**

You can render the shader to an off-screen buffer and save it as an image:

-   **Pillow**: Save framebuffer as an image file.
-   Example:

    ```python
    from PIL import Image
    import numpy as np

    # Suppose `pixels` is a numpy array from the framebuffer
    img = Image.fromarray(pixels, 'RGB')
    img.save('shader_wallpaper.png')
    ```

---

### 5. **Build a GUI (Optional)**

For user interaction (changing colors, patterns, etc.):

-   Use **Tkinter** or **PyQt** to create a GUI for selecting shader parameters and generating wallpapers dynamically.

---

### Tools and Examples:

-   **shadertoy.com**: Test GLSL shaders and adapt them for your Python app.
-   **glumpy**: A Python library for fast OpenGL rendering, including shaders.

---

### Minimal Example with ModernGL:

```python
import moderngl
import numpy as np
from PIL import Image

# Initialize ModernGL context
ctx = moderngl.create_standalone_context()

# Create framebuffer
width, height = 1920, 1080
fbo = ctx.framebuffer(color_attachments=[ctx.texture((width, height), 4)])

# Shader program
prog = ctx.program(
    vertex_shader="""
    #version 330
    in vec2 in_vert;
    void main() {
        gl_Position = vec4(in_vert, 0.0, 1.0);
    }
    """,
    fragment_shader="""
    #version 330
    out vec4 FragColor;
    void main() {
        vec2 uv = gl_FragCoord.xy / vec2(1920, 1080);
        FragColor = vec4(uv, 0.5, 1.0);
    }
    """,
)

# Render a quad
quad = ctx.buffer(np.array([-1.0, -1.0, 1.0, -1.0, -1.0, 1.0, 1.0, 1.0], dtype='f4'))
vao = ctx.simple_vertex_array(prog, quad, 'in_vert')
fbo.use()
vao.render(moderngl.TRIANGLE_STRIP)

# Save to file
data = fbo.read(components=3)
img = Image.frombytes('RGB', (width, height), data)
img = img.transpose(Image.FLIP_TOP_BOTTOM)  # OpenGL origin is bottom-left
img.save('wallpaper.png')
```

---

This code generates a gradient wallpaper, but you can replace the fragment shader with more complex effects.
