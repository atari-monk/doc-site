Here are templates for defining and using enums in modern Python using the `Enum` module from the `enum` standard library.

### Basic Enum Definition
```python
from enum import Enum

class ExampleEnum(Enum):
    VALUE_ONE = 1
    VALUE_TWO = 2
    VALUE_THREE = 3
```

### Enum with String Values
```python
from enum import Enum

class StringEnum(Enum):
    VALUE_ONE = "one"
    VALUE_TWO = "two"
    VALUE_THREE = "three"
```

### Enum with Descriptions (using `auto()` and `Enum` methods)
```python
from enum import Enum, auto

class DescriptiveEnum(Enum):
    VALUE_ONE = auto()
    VALUE_TWO = auto()
    VALUE_THREE = auto()

    def describe(self):
        return f"This is {self.name} with value {self.value}"
```

### Enum with Methods
```python
from enum import Enum

class ActionEnum(Enum):
    START = "start"
    STOP = "stop"
    PAUSE = "pause"

    def perform_action(self):
        if self == ActionEnum.START:
            return "Starting..."
        elif self == ActionEnum.STOP:
            return "Stopping..."
        elif self == ActionEnum.PAUSE:
            return "Pausing..."
```

### Enum with Custom Attributes
```python
from enum import Enum

class StatusEnum(Enum):
    SUCCESS = (1, "Operation was successful")
    FAILURE = (0, "Operation failed")
    PENDING = (2, "Operation is pending")

    def __init__(self, code, message):
        self.code = code
        self.message = message

# Usage Example:
# print(StatusEnum.SUCCESS.message)  # Output: Operation was successful
```

### Enum with `IntEnum` (for integer-based enums)
```python
from enum import IntEnum

class LevelEnum(IntEnum):
    LOW = 1
    MEDIUM = 2
    HIGH = 3
```

### Enum with Flag (for bitwise operations)
```python
from enum import Flag, auto

class PermissionFlag(Flag):
    READ = auto()
    WRITE = auto()
    EXECUTE = auto()

# Usage Example:
# combined = PermissionFlag.READ | PermissionFlag.WRITE
# print(combined)  # Output: PermissionFlag.READ|WRITE
```

You can include any of these templates in your documentation to reuse in your projects! Let me know if you'd like more specific examples or additional variations.

## My examples

```python
from enum import Enum

class StorageKey(Enum):
    APP_DATA_FOLDER = "app_data_folder"
    KEY_VALUE_STORAGE_PATH = "key_value_storage_path"
    VID_TO_MP3_SAVE_FOLDER = "vid_to_mp3_save_folder"
    DOC_SITE_DATA_FOLDER = "doc_site_data_folder"
    SCENE_FILE_PATH = "scene_file_path"
```
