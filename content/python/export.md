# Export for lib/package

## What and how to export from `__init__.py` so i can import it in other package ?

Using example of cli_logger module with logger.py file containing function setup_logger, class LoggerConfig.

To allow importing the `setup_logger` function and `LoggerConfig` class from `logger.py` in other packages, you need to ensure they are properly exported from the `__init__.py` file of the package.

### Steps:

1. Ensure that `__init__.py` is present in the package folder where `logger.py` resides.
2. Inside `__init__.py`, import the components you want to make accessible from the package (in this case, `setup_logger` and `LoggerConfig`).

### Example structure:

```
my_package/
    __init__.py
    logger.py
```

### In `__init__.py`, add the following:

```python
from .logger import setup_logger, LoggerConfig
```

### Now, you can import `setup_logger` and `LoggerConfig` in other packages as follows:

```python
from my_package import setup_logger, LoggerConfig
```

### Additional Notes:

-   The `setup_logger` function will allow you to set up a logger in your other modules.
-   The `LoggerConfig` class is an `Enum` that defines various configuration options for the logger, so you can use these values to configure the logger.
