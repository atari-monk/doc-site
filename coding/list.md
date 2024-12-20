# List of projects by atari monk

## DOC SITE

Project building web page on github pages.  
Uses markdown. Notes database.
Simple, minimal structure by design.

**Repositories:**

-   [doc-site](https://github.com/atari-monk/doc-site)
    Main repository with github pages configured in root folder.

## CLI TOOL

Project building console line interface tool in python.  
While loop with commands.  
Commands are imported from pip packages.

**Repositories:**

-   [cli_tool](https://github.com/atari-monk/cli_tool)
    Main repository with cli tool.
-   [keyval_storage](https://github.com/atari-monk/keyval_storage)
    Tool to store key value pairs used in cli tool and its commands.
-   [cli_commands](https://github.com/atari-monk/cli_commands)
    Set of commands for cli tool.
-   [cli_logger](https://github.com/atari-monk/cli_logger)
    Logger for cli environment.
-   [pytoolbox](https://github.com/atari-monk/pytoolbox)
    Small abstractions so i dont have to remember function details.

### Relations between packages

1. Layer 1, independent packages, based only on python.

-   pytoolbox
-   keyval_storage
-   cli_logger

2. Layer 2, composite packages, based on python, level 1 and optionally third party pip packages.

-   cli_tool
    dependencies: pytoolbox, keyval_storage, cli_logger
-   cli_commands
    dependencies: pytoolbox, keyval_storage, cli_logger
