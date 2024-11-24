# Formatter class

Let's break it down to clarify what `formatter_class` does in this context and why it might be useful. Here's a simplified explanation:

---

### What is a Formatter?

-   A **formatter** in Python's `logging` module is responsible for formatting log messages. It specifies how the log message will look when it's written to the console or a file.
-   By default, you use `logging.Formatter` with a string format like `'%(asctime)s - %(name)s - %(levelname)s - %(message)s'`.

---

### Why Use `formatter_class`?

-   The `formatter_class` allows you to **customize the formatting behavior**. For example:
    -   You might want a JSON formatter instead of plain text.
    -   Or, you might need a custom class to add extra metadata or modify how log levels are displayed.

Here's how you might use `formatter_class`:

```python
formatter_class = config.get('formatter_class', logging.Formatter)
formatter = formatter_class(config[LoggerConfig.FORMAT])
```

### Step-by-Step Explanation:

1. **`config.get('formatter_class', logging.Formatter)`**:

    - This retrieves the formatter class specified in the configuration.
    - If no custom class is provided, it defaults to `logging.Formatter`.

2. **`formatter_class(config[LoggerConfig.FORMAT])`**:
    - Creates an instance of the formatter class (e.g., `logging.Formatter`) using the format string from the configuration.

---

### Why This Design is Useful:

1. **Flexibility**:

    - By allowing the user to specify `formatter_class`, the logger setup becomes more flexible.
    - You can use different formatters for different purposes without modifying the logger code.

2. **Fallback**:
    - If the user doesn't specify a custom `formatter_class`, the logger still works with the default `logging.Formatter`.

---

### Example of a Custom Formatter

Suppose you want your logs in JSON format. You can define a custom formatter:

```python
import json

class JSONFormatter(logging.Formatter):
    def format(self, record: logging.LogRecord) -> str:
        log_message = {
            "timestamp": self.formatTime(record),
            "level": record.levelname,
            "name": record.name,
            "message": record.getMessage(),
        }
        return json.dumps(log_message)

# Use it in the config
config = {
    LoggerConfig.FORMAT: '%(asctime)s - %(name)s - %(levelname)s - %(message)s',
    'formatter_class': JSONFormatter
}
```

When the logger is created, it will use the `JSONFormatter` to format all log messages in JSON format instead of plain text.

---

### Final Code with `formatter_class`

```python
formatter_class = config.get('formatter_class', logging.Formatter)
formatter = formatter_class(config[LoggerConfig.FORMAT])  # Creates a formatter instance

console_handler = logging.StreamHandler()
console_handler.setLevel(config[LoggerConfig.CONSOLE_LEVEL])
console_handler.setFormatter(formatter)
logger.addHandler(console_handler)
```

### Summary

Using `formatter_class` in this way allows the logger to dynamically adapt to different formatting needs. If you're not working with custom formatters yet, it's fine to skip it, but keeping this design makes your code future-proof!
