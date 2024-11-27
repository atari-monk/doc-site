# Log

## 2024.11.24

### 18:19

I spent most of my time working on the **_cli tool_** alone and didn't integrate py frameworks for the CLI commands.  
I created **_key value storage_**.  
I can't create a separate repository for every command; many of them will need to be bundled into a single repo,  
with some commands having their own repos. For now, I don’t want to focus on frameworks,  
as my CLI seems to be working in some capacity.  
**_Plan_:** Next, I want to add a repository for file-based data, and the CLI can have its key value storage there.  
I want to move the log command into my current CLI tool setup.

### 22:56

I created functions for **_loading and saving JSON_** in pytoolbox, using them to abstract the code so I don't have to remember it.  
I also set up a **_class for key-value storage_** in cli_tool.  
I'm currently making these packages installable via pip, along with the functions I created.  
Regardless of whether this project is truly necessary or fully logical, I enjoyed working on it.

## 2024.11.25

### 08:54

Note on **_package hierarchy_** and decided where to move Storage Provider class.

### 12:40

From old Storage Provider, created new Storage Provider and Config Provider.  
These are layer on top of tool functions; for key value type files.  
Config is used to store path for key value storage file.  
Key Value storage is some api (crud methods) on json.
Hope was storage would be usefull for cli command params that can use memory.

### 15:01

Layer 1 - tool functions.  
Layer 2 - using then with config type data.  
Layer 3 - using layer 3 methods, orchestrating them.
This was in context of config/key-value storage files.
Next: Move layer 3 code to its class.  
Use it in 2 packages.

### 16:24

ConfigStorageInteraction class in lib and used in first package.

### 18:10

ConfigStorageInteraction used in secod package.
