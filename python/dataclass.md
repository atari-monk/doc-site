# What is @dataclass in py

In Python, `@dataclass` is a decorator from the `dataclasses` module that simplifies the creation of classes designed to store data. It automates the generation of common methods such as `__init__`, `__repr__`, `__eq__`, and others, based on the class attributes.

### Key Features:

1. **Automatic `__init__`**: Generates an initializer that takes arguments for all fields defined in the class.
2. **Readable `__repr__`**: Provides a string representation of the object, useful for debugging.
3. **Equality with `__eq__`**: Automatically compares objects based on their fields.
4. **Mutable or Immutable**: You can control mutability using the `frozen` parameter.
5. **Default Values and Factories**: Supports default values for fields or factories for more complex defaults (e.g., lists or dicts).

### Example:

```python
from dataclasses import dataclass

@dataclass
class Point:
    x: int
    y: int

# Usage
p1 = Point(3, 4)
p2 = Point(3, 4)
print(p1)          # Output: Point(x=3, y=4)
print(p1 == p2)    # Output: True (compares field values)
```

### Adding Defaults and Other Features:

```python
from dataclasses import dataclass, field

@dataclass
class Employee:
    name: str
    age: int = 30  # Default value
    roles: list = field(default_factory=list)  # Default factory for mutable types

# Usage
e1 = Employee("Alice")
print(e1)  # Output: Employee(name='Alice', age=30, roles=[])
```

### Parameters of `@dataclass`:

-   `init`: Whether to generate an `__init__` method (default is `True`).
-   `repr`: Whether to generate a `__repr__` method (default is `True`).
-   `eq`: Whether to generate an `__eq__` method (default is `True`).
-   `order`: Whether to generate ordering methods like `__lt__`, `__gt__`, etc. (default is `False`).
-   `frozen`: Makes the instance immutable if set to `True`.
-   `unsafe_hash`: Allows generating a hash function even if the class is mutable.

### Example of an Immutable Dataclass:

```python
@dataclass(frozen=True)
class Color:
    red: int
    green: int
    blue: int

# Usage
c = Color(255, 0, 0)
print(c)           # Output: Color(red=255, green=0, blue=0)
# c.red = 0        # This will raise a FrozenInstanceError
```
