# Lambda

Yes, `lambda` functions can take arguments. The syntax for a `lambda` function with arguments is as follows:

```python
lambda arg1, arg2, ...: expression
```

For example, if you want a `lambda` to log a message passed as an argument:

```python
def load():
    return {
        "ping": lambda msg: logger.info(msg),
    }
```

Then you can call the `ping` key in the dictionary like this:

```python
load()["ping"]("ping message here")
```

This will log the message `"ping message here"`.
