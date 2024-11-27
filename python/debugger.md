# How to add debugger to py code ?

Adding a debugger to Python code can help you inspect variables, step through the code, and identify issues. Here are several ways to incorporate debugging into your Python scripts:

---

### 1. **Using `pdb` (Python Debugger)**

The built-in `pdb` module is a powerful tool for debugging Python code.

-   **Add a Breakpoint:**
    Insert the following line in your code where you want to pause execution:

    ```python
    import pdb; pdb.set_trace()
    ```

-   **Run the Code:**
    When execution reaches the breakpoint, you'll enter an interactive debugging shell where you can inspect variables, step through the code, and continue or quit.

-   **Commands in `pdb`:**
    -   `n` (next): Execute the next line of code.
    -   `s` (step): Step into a function.
    -   `c` (continue): Resume execution until the next breakpoint.
    -   `q` (quit): Exit the debugger.

---

### 2. **Using a Debugger in an IDE**

Modern IDEs and text editors have integrated debuggers that make debugging easier.

-   **VS Code:**

    1. Open your Python file.
    2. Click the "Run and Debug" button or press `F5`.
    3. Set breakpoints by clicking in the left margin of the editor.
    4. Use the debug controls to step through your code.

-   **PyCharm:**
    1. Open your project in PyCharm.
    2. Right-click a line and select "Toggle Breakpoint" to set a breakpoint.
    3. Click the "Debug" button or select "Run > Debug."

---

### 3. **Using `ipdb` for Improved Debugging**

`ipdb` provides a more user-friendly debugging interface compared to `pdb`.

-   **Install ipdb:**

    ```bash
    pip install ipdb
    ```

-   **Use ipdb:**
    Replace the `pdb` import with:
    ```python
    import ipdb; ipdb.set_trace()
    ```

---

### 4. **Using `breakpoint()` in Python 3.7+**

Python 3.7 introduced the `breakpoint()` function as a shortcut for invoking the debugger.

-   **Add a Breakpoint:**

    ```python
    breakpoint()
    ```

-   **Set the Debugger:**
    By default, it uses `pdb`. You can customize the debugger by setting the `PYTHONBREAKPOINT` environment variable:
    ```bash
    export PYTHONBREAKPOINT=ipdb.set_trace
    ```

---

### 5. **Debugging Scripts Directly**

You can run your script in debugging mode from the command line:

```bash
python -m pdb your_script.py
```

This starts your script in the debugger, allowing you to step through the code from the start.

---

### Example:

Here’s a small example to debug a Python script:

```python
def calculate_area(radius):
    area = 3.14 * radius ** 2
    return area

radius = 5
import pdb; pdb.set_trace()  # Debugger starts here
result = calculate_area(radius)
print(f"The area is {result}")
```

When you run this, the debugger pauses at `pdb.set_trace()` and allows you to inspect `radius` or step through the function.

---

### Tips:

-   Use meaningful breakpoints near potential issues.
-   Keep an eye on variable values and program flow during debugging.
-   For large projects, combine debugging tools with logging to capture context.
