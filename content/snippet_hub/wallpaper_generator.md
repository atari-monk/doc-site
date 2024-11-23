# Wallpaper Generator

## Write me an application to generate 1920x1080 wallpaper, vector art with transparent background. Where we can add different fun algorithms producing image.

Here is a Python application using the **`Pillow`** library to create 1920x1080 transparent vector-like wallpapers with fun algorithms for generating images. You can expand it by adding your custom image-generation functions.

---

### Install Required Library

First, ensure you have the Pillow library installed:

```bash
pip install pillow
```

---

### Application Code

```python
import os
from PIL import Image, ImageDraw
import random


class WallpaperGenerator:
    def __init__(self, width=1920, height=1080):
        self.width = width
        self.height = height
        self.image = Image.new("RGBA", (self.width, self.height), (0, 0, 0, 0))
        self.draw = ImageDraw.Draw(self.image)

    def clear_image(self):
        """Reset the image to a blank transparent state."""
        self.image = Image.new("RGBA", (self.width, self.height), (0, 0, 0, 0))
        self.draw = ImageDraw.Draw(self.image)

    def algorithm_circles(self):
        """Generate random circles."""
        for _ in range(50):  # Number of circles
            x = random.randint(0, self.width)
            y = random.randint(0, self.height)
            r = random.randint(20, 200)  # Random radius
            color = (
                random.randint(0, 255),
                random.randint(0, 255),
                random.randint(0, 255),
                random.randint(50, 200),  # Semi-transparent
            )
            self.draw.ellipse((x - r, y - r, x + r, y + r), fill=color)

    def algorithm_lines(self):
        """Generate random intersecting lines."""
        for _ in range(100):  # Number of lines
            x1 = random.randint(0, self.width)
            y1 = random.randint(0, self.height)
            x2 = random.randint(0, self.width)
            y2 = random.randint(0, self.height)
            color = (
                random.randint(0, 255),
                random.randint(0, 255),
                random.randint(0, 255),
                random.randint(50, 200),  # Semi-transparent
            )
            self.draw.line((x1, y1, x2, y2), fill=color, width=random.randint(1, 5))

    def algorithm_rectangles(self):
        """Generate random rectangles."""
        for _ in range(30):  # Number of rectangles
            x1 = random.randint(0, self.width)
            y1 = random.randint(0, self.height)
            x2 = random.randint(0, self.width)
            y2 = random.randint(0, self.height)
            color = (
                random.randint(0, 255),
                random.randint(0, 255),
                random.randint(0, 255),
                random.randint(50, 200),  # Semi-transparent
            )
            self.draw.rectangle((x1, y1, x2, y2), fill=color)

    def save_image(self, file_name="wallpaper.png"):
        """Save the generated image to a file."""
        os.makedirs("output", exist_ok=True)
        file_path = os.path.join("output", file_name)
        self.image.save(file_path, "PNG")
        print(f"Wallpaper saved to {file_path}")


def main():
    generator = WallpaperGenerator()

    # Choose an algorithm
    print("Choose an algorithm:")
    print("1. Random Circles")
    print("2. Random Lines")
    print("3. Random Rectangles")
    choice = input("Enter the number of the algorithm (1-3): ")

    # Apply the chosen algorithm
    if choice == "1":
        generator.algorithm_circles()
    elif choice == "2":
        generator.algorithm_lines()
    elif choice == "3":
        generator.algorithm_rectangles()
    else:
        print("Invalid choice. Exiting.")
        return

    # Save the generated wallpaper
    generator.save_image()


if __name__ == "__main__":
    main()
```

---

### Features

1. **Random Circles:** Creates colorful, semi-transparent overlapping circles.
2. **Random Lines:** Generates intersecting lines with varying thickness and transparency.
3. **Random Rectangles:** Produces random-sized and semi-transparent rectangles.

---

### Running the Application

1. Save the script as `wallpaper_generator.py`.
2. Run it:
    ```bash
    python wallpaper_generator.py
    ```
3. Choose the desired algorithm (1, 2, or 3).
4. The wallpaper will be saved in the `output` directory as `wallpaper.png`.

---

### Adding More Algorithms

To add new image-generation algorithms, define a new method in the `WallpaperGenerator` class. For example:

```python
def algorithm_spirals(self):
    """Generate a spiral pattern."""
    for i in range(100):
        angle = i * 15
        x = self.width // 2 + int(i * 5 * random.uniform(0.8, 1.2)) * math.cos(math.radians(angle))
        y = self.height // 2 + int(i * 5 * random.uniform(0.8, 1.2)) * math.sin(math.radians(angle))
        color = (random.randint(0, 255), random.randint(0, 255), random.randint(0, 255), 100)
        self.draw.ellipse((x - 10, y - 10, x + 10, y + 10), fill=color)
```

This allows you to expand functionality easily.
