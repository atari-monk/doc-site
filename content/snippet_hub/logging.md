# Python logging in cli context.

This setup has features:

-   name of module where it is used allows to identiffy file
-   logging to console and rotating file (10 MB, 3 files stored)
-   logging level for console and file independently
-   format '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
-   config dictionary

## This snippet is my current logging tool.

```python
#logger.py

import logging
import os
from typing import Optional
from logging.handlers import RotatingFileHandler

def setup_logger(name: str, config: Optional[dict[str, Optional[object]]]=None) -> logging.Logger:
if config is None:
config = {
'log_file': 'logs/app.log',
'log_to_file': True,
'mainLevel': logging.DEBUG,
'consoleLevel': logging.DEBUG,
'fileLevel': logging.DEBUG,
'format': '%(asctime)s - %(name)s - %(levelname)s - %(message)s',
'max_bytes': 10 * 1024 * 1024, # 10 MB, you can adjust this
'backup_count': 3, # Number of backup files to keep
}

    logger = logging.getLogger(name)
    logger.setLevel(config.get('mainLevel', logging.DEBUG))

    if not logger.hasHandlers():
        # Console handler setup
        console_handler = logging.StreamHandler()
        console_handler.setLevel(config.get('consoleLevel', logging.DEBUG))
        console_formatter = logging.Formatter(config.get('format', '%(asctime)s - %(name)s - %(levelname)s - %(message)s'))
        console_handler.setFormatter(console_formatter)
        logger.addHandler(console_handler)

        # File handler setup with rotation
        if config.get('log_to_file', True):
            log_file = config.get('log_file', 'logs/app.log')
            if os.path.dirname(log_file):
                os.makedirs(os.path.dirname(log_file), exist_ok=True)

            # Rotating file handler setup
            file_handler = RotatingFileHandler(
                log_file,
                maxBytes=config.get('max_bytes', 10 * 1024 * 1024),  # Default 10MB
                backupCount=config.get('backup_count', 3)             # Default 5 backups
            )
            file_handler.setLevel(config.get('fileLevel', logging.DEBUG))
            file_formatter = logging.Formatter(config.get('format', '%(asctime)s - %(name)s - %(levelname)s - %(message)s'))
            file_handler.setFormatter(file_formatter)
            logger.addHandler(file_handler)

    return logger
```

## This is a data model for logger config.

```python
import logging

LOGGER_CONFIG = {
            'log_file': 'logs/cli_app.log',
            'log_to_file': True,
            'mainLevel': logging.INFO,
            'consoleLevel': logging.INFO,
            'fileLevel': logging.INFO,
            'format': '%(asctime)s - %(name)s - %(levelname)s - %(message)s',
            'max_bytes': 10 * 1024 * 1024,  # 10 MB, you can adjust this
            'backup_count': 3,
        }
```

## This is a usage of logger in module.

```python
#some_module.py

from shared.logger import setup_logger

logger = setup_logger(__name__, LOGGER_CONFIG)

```

## Strengths:

1. **Module Identification**:

    - The use of the `name` parameter ensures that log entries can identify which module produced them. This is critical in applications with multiple modules.

2. **Configurable Logging**:

    - A `config` dictionary allows flexibility to adjust logging settings without modifying the core `setup_logger` function.

3. **Console and File Logging**:

    - The dual logging system ensures real-time visibility via the console and persistence for later debugging via files.

4. **Rotating File Handler**:

    - Rotating logs avoid bloated files and maintain a history with backups.

5. **Adjustable Log Levels**:

    - Independent log levels for the console and file handlers are a great touch for development versus production settings.

6. **Format String**:
    - The log format includes timestamp, module name, level, and message—perfect for debugging.
