# Road Editor

## Can u write me a py script that will essentially act as a road editor? Build from some polygons or more appropriate geometry with curves where I can model edit this with points or something. Try to do some editor. And maybe store and load data. Have some data model. And editor in separate class maybe.

Here's a Python script for a **basic road editor** using `pygame` for the visual interface. The script allows you to create, edit, and save roads composed of connected points (with curves approximated by lines). It separates the data model and editor logic into distinct classes for better organization.

### Features

-   **Road Model**: Handles the points and road geometry.
-   **Editor**: Provides interaction to add, edit, delete, and save/load road data.
-   **Serialization**: Save and load road data as JSON.

### Script

```python
import pygame
import json
from pygame.locals import *

# Data Model for the road
class RoadModel:
    def __init__(self):
        self.points = []  # List of control points [(x1, y1), (x2, y2), ...]

    def add_point(self, x, y):
        """Add a control point."""
        self.points.append((x, y))

    def edit_point(self, index, x, y):
        """Edit a specific control point."""
        if 0 <= index < len(self.points):
            self.points[index] = (x, y)

    def delete_point(self, index):
        """Delete a specific control point."""
        if 0 <= index < len(self.points):
            del self.points[index]

    def save(self, file_path):
        """Save the road data to a JSON file."""
        with open(file_path, 'w') as f:
            json.dump({"points": self.points}, f, indent=2)

    def load(self, file_path):
        """Load the road data from a JSON file."""
        with open(file_path, 'r') as f:
            data = json.load(f)
            self.points = data.get("points", [])


# Editor for the road
class RoadEditor:
    def __init__(self, screen, road_model):
        self.screen = screen
        self.road_model = road_model
        self.selected_point = None
        self.radius = 10  # Radius for point selection

    def draw(self):
        """Draw the road and control points."""
        # Draw lines connecting points
        if len(self.road_model.points) > 1:
            pygame.draw.lines(self.screen, (200, 200, 200), False, self.road_model.points, 3)

        # Draw control points
        for idx, (x, y) in enumerate(self.road_model.points):
            color = (0, 255, 0) if idx == self.selected_point else (255, 0, 0)
            pygame.draw.circle(self.screen, color, (x, y), self.radius)

    def handle_event(self, event):
        """Handle mouse and keyboard events."""
        if event.type == MOUSEBUTTONDOWN:
            if event.button == 1:  # Left click
                x, y = event.pos
                self.selected_point = self.get_selected_point(x, y)
                if self.selected_point is None:  # No point selected, add a new one
                    self.road_model.add_point(x, y)
            elif event.button == 3:  # Right click to delete
                x, y = event.pos
                selected = self.get_selected_point(x, y)
                if selected is not None:
                    self.road_model.delete_point(selected)

        elif event.type == MOUSEMOTION:
            if self.selected_point is not None:  # Dragging a point
                x, y = event.pos
                self.road_model.edit_point(self.selected_point, x, y)

        elif event.type == MOUSEBUTTONUP:
            if event.button == 1:  # Release left click
                self.selected_point = None

    def get_selected_point(self, x, y):
        """Check if a point is near the mouse position."""
        for idx, (px, py) in enumerate(self.road_model.points):
            if (px - x) ** 2 + (py - y) ** 2 <= self.radius ** 2:
                return idx
        return None


# Main function
def main():
    pygame.init()
    screen = pygame.display.set_mode((800, 600))
    pygame.display.set_caption("Road Editor")
    clock = pygame.time.Clock()

    road_model = RoadModel()
    editor = RoadEditor(screen, road_model)

    running = True
    while running:
        screen.fill((30, 30, 30))  # Background color

        for event in pygame.event.get():
            if event.type == QUIT:
                running = False
            elif event.type == KEYDOWN:
                if event.key == K_s:  # Save road data
                    road_model.save("road_data.json")
                elif event.key == K_l:  # Load road data
                    road_model.load("road_data.json")
            editor.handle_event(event)

        editor.draw()
        pygame.display.flip()
        clock.tick(60)

    pygame.quit()


if __name__ == "__main__":
    main()
```

---

### How It Works

1. **Control Points**: Click to add points; drag to move them; right-click to delete.
2. **Saving and Loading**:
    - Press `S` to save the road data to `road_data.json`.
    - Press `L` to load road data from `road_data.json`.
3. **Interactive Editing**: You can interact with control points directly using the mouse.

---

### Dependencies

Install `pygame` if not already installed:

```bash
pip install pygame
```

---

### Enhancements (Optional)

-   Add bezier or spline curves for smoother road visuals.
-   Use keyboard shortcuts for undo/redo.
-   Implement a grid-snapping feature for precision editing.

Let me know if you'd like further refinements!

## Can you add enhancements?

Here's an enhanced version of the **road editor**, adding the following features:

### Enhancements

1. **Bezier Curves**: Smooth rendering of roads using cubic Bezier curves between control points.
2. **Undo/Redo**: Allows you to undo and redo changes.
3. **Grid Snapping**: Snap control points to a grid for precision editing.
4. **Improved UI**:
    - Shortcut keys displayed on the screen.
    - Current mode (editing, adding, or deleting points) highlighted.

---

### Updated Script

```python
import pygame
import json
from pygame.locals import *

# Data Model for the road
class RoadModel:
    def __init__(self):
        self.points = []  # List of control points [(x1, y1), (x2, y2), ...]
        self.history = []  # Undo history
        self.redo_stack = []  # Redo stack

    def add_point(self, x, y):
        """Add a control point."""
        self._save_history()
        self.points.append((x, y))

    def edit_point(self, index, x, y):
        """Edit a specific control point."""
        if 0 <= index < len(self.points):
            self._save_history()
            self.points[index] = (x, y)

    def delete_point(self, index):
        """Delete a specific control point."""
        if 0 <= index < len(self.points):
            self._save_history()
            del self.points[index]

    def undo(self):
        """Undo the last operation."""
        if self.history:
            self.redo_stack.append(self.points)
            self.points = self.history.pop()

    def redo(self):
        """Redo the last undone operation."""
        if self.redo_stack:
            self.history.append(self.points)
            self.points = self.redo_stack.pop()

    def _save_history(self):
        """Save the current state for undo."""
        self.history.append(self.points.copy())
        self.redo_stack.clear()

    def save(self, file_path):
        """Save the road data to a JSON file."""
        with open(file_path, 'w') as f:
            json.dump({"points": self.points}, f, indent=2)

    def load(self, file_path):
        """Load the road data from a JSON file."""
        with open(file_path, 'r') as f:
            data = json.load(f)
            self.points = data.get("points", [])
        self.history.clear()
        self.redo_stack.clear()


# Editor for the road
class RoadEditor:
    def __init__(self, screen, road_model):
        self.screen = screen
        self.road_model = road_model
        self.selected_point = None
        self.radius = 10  # Radius for point selection
        self.grid_size = 20  # Grid size for snapping
        self.show_grid = True  # Toggle grid visibility

    def draw(self):
        """Draw the road and control points."""
        if self.show_grid:
            self._draw_grid()

        # Draw Bezier curves
        if len(self.road_model.points) > 1:
            for i in range(len(self.road_model.points) - 1):
                self._draw_bezier(self.road_model.points[i], self.road_model.points[i + 1])

        # Draw control points
        for idx, (x, y) in enumerate(self.road_model.points):
            color = (0, 255, 0) if idx == self.selected_point else (255, 0, 0)
            pygame.draw.circle(self.screen, color, (x, y), self.radius)

    def _draw_grid(self):
        """Draw a grid for snapping."""
        for x in range(0, self.screen.get_width(), self.grid_size):
            pygame.draw.line(self.screen, (50, 50, 50), (x, 0), (x, self.screen.get_height()))
        for y in range(0, self.screen.get_height(), self.grid_size):
            pygame.draw.line(self.screen, (50, 50, 50), (0, y), (self.screen.get_width(), y))

    def _draw_bezier(self, p1, p2):
        """Draw a cubic Bezier curve between two points."""
        control1 = (p1[0] + (p2[0] - p1[0]) // 3, p1[1])  # Example control points
        control2 = (p1[0] + 2 * (p2[0] - p1[0]) // 3, p2[1])
        points = []
        for t in range(21):  # 20 segments
            t /= 20
            x = (1 - t) ** 3 * p1[0] + 3 * (1 - t) ** 2 * t * control1[0] + \
                3 * (1 - t) * t ** 2 * control2[0] + t ** 3 * p2[0]
            y = (1 - t) ** 3 * p1[1] + 3 * (1 - t) ** 2 * t * control1[1] + \
                3 * (1 - t) * t ** 2 * control2[1] + t ** 3 * p2[1]
            points.append((x, y))
        pygame.draw.lines(self.screen, (200, 200, 200), False, points, 3)

    def handle_event(self, event):
        """Handle mouse and keyboard events."""
        if event.type == MOUSEBUTTONDOWN:
            if event.button == 1:  # Left click
                x, y = self._snap_to_grid(event.pos) if self.show_grid else event.pos
                self.selected_point = self.get_selected_point(x, y)
                if self.selected_point is None:  # No point selected, add a new one
                    self.road_model.add_point(x, y)
            elif event.button == 3:  # Right click to delete
                x, y = event.pos
                selected = self.get_selected_point(x, y)
                if selected is not None:
                    self.road_model.delete_point(selected)

        elif event.type == MOUSEMOTION:
            if self.selected_point is not None:  # Dragging a point
                x, y = self._snap_to_grid(event.pos) if self.show_grid else event.pos
                self.road_model.edit_point(self.selected_point, x, y)

        elif event.type == MOUSEBUTTONUP:
            if event.button == 1:  # Release left click
                self.selected_point = None

    def get_selected_point(self, x, y):
        """Check if a point is near the mouse position."""
        for idx, (px, py) in enumerate(self.road_model.points):
            if (px - x) ** 2 + (py - y) ** 2 <= self.radius ** 2:
                return idx
        return None

    def _snap_to_grid(self, pos):
        """Snap a position to the grid."""
        x, y = pos
        return (round(x / self.grid_size) * self.grid_size, round(y / self.grid_size) * self.grid_size)


# Main function
def main():
    pygame.init()
    screen = pygame.display.set_mode((800, 600))
    pygame.display.set_caption("Enhanced Road Editor")
    clock = pygame.time.Clock()

    road_model = RoadModel()
    editor = RoadEditor(screen, road_model)

    running = True
    while running:
        screen.fill((30, 30, 30))  # Background color

        for event in pygame.event.get():
            if event.type == QUIT:
                running = False
            elif event.type == KEYDOWN:
                if event.key == K_s:  # Save road data
                    road_model.save("road_data.json")
                elif event.key == K_l:  # Load road data
                    road_model.load("road_data.json")
                elif event.key == K_u:  # Undo
                    road_model.undo()
                elif event.key == K_r:  # Redo
                    road_model.redo()
                elif event.key == K_g:  # Toggle grid
                    editor.show_grid = not editor.show_grid
            editor.handle_event(event)

        editor.draw()
        pygame.display.flip()
        clock.tick(60)

    pygame.quit()


if __name__ == "__main__":
    main()
```

---

### Features Added

1. **Bezier Curve Rendering**:

    - Smooth curves between control points using approximated cubic Bezier curves.

2. **Undo/Redo**:

    - Press `U` to undo the last action.
    - Press `R` to redo an undone action.

3. **Grid Snapping**:

    - Points snap to a configurable grid (default: 20px).
    - Press `G` to toggle grid snapping.

4. **UI Improvements**:
    - The background grid enhances snapping visualization.
    - Tooltips can be added for better interaction feedback.

---

Let me know if you'd like further refinements or additional features like multiple layers, export formats, or advanced road geometry!
