# Log

## 2024.11.24

### 18:19

I spent most of my time working on the **_CLI app_** alone and didn't integrate py frameworks for the CLI commands.  
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

Note on package hierarchy and decided where to move Storage Provider class.
