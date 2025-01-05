## https://stackoverflow.com/questions/24777289/what-is-in-makefile


#### The typical way to compile a gcc file is to g++ file.cc -o main, which creates an executable file of name main.out whic can be executed using ./main.out, with makefile we can compile multiple .cc files to object files .o, then we can call the linker to link multiple object files to create an executable, which is .out in unix and .exe in windows. Same thing can be achieved using g++ command but it's tiresome to do it manually, also makefiles only build the executable if the source files are actually changed.

``
source files [.cc, .h] -----compiling---> object files [.o] (intermidiery files) ------linking-----> executable file [.out or .exe] (final product) 
``

``
?= indicates to set the KDIR variable only if it's not set/doesn't have a value.

For example:

KDIR ?= "foo"
KDIR ?= "bar"

test:
    echo $(KDIR)

``




``

In Makefiles, := and = are used to assign values to variables, but they have different behaviors:
:= (Simple Assignment)
 * Immediate Evaluation: The value on the right-hand side is evaluated and assigned to the variable immediately.
 * No Recursive Expansion: Variables on the right-hand side are not expanded during the initial assignment. They are expanded later when the variable is used.
 * Example:
VAR := $(shell date)

In this case, $(shell date) is not executed immediately. Instead, VAR is assigned the literal string $(shell date). When VAR is used later, $(shell date) is executed, and the result is substituted.
= (Recursive Assignment)
 * Delayed Evaluation: The value on the right-hand side is not evaluated immediately. It is evaluated each time the variable is used.
 * Recursive Expansion: Variables on the right-hand side are expanded during each evaluation. This allows for circular dependencies and complex variable definitions.
 * Example:
VAR = $(shell date)

Here, $(shell date) is executed every time VAR is used. This can lead to different values being assigned to VAR depending on when it is used.
Key Differences
| Feature | := (Simple Assignment) | = (Recursive Assignment) |
|---|---|---|
| Evaluation | Immediate | Delayed |
| Expansion | No recursive expansion | Recursive expansion |
| Circular Dependencies | Not allowed | Allowed |
Choosing Between := and =
 * Use := for most cases, as it is generally more predictable and efficient.
 * Use = when you need recursive variable expansion or circular dependencies.
By understanding these differences, you can write more efficient and maintainable Makefiles.

``

``

.PHONY

By default, Makefile targets are "file targets" - they are used to build files from other files. Make assumes its target is a file, and this makes writing Makefiles relatively easy:

foo: bar
  create_one_from_the_other foo bar

However, sometimes, you want your Makefile to run commands that do not represent physical files in the file system. Good examples of this are the common targets "clean" and "all". Chances are this isn't the case, but you may potentially have a file named clean in your main directory. In such a case Make will be confused because by default the clean target would be associated with this file and Make will only run it when the file doesn't appear to be up-to-date with regards to its dependencies.

These special targets are called phony and you can explicitly tell Make they're not associated with files, e.g.:

.PHONY: clean


``

# Cpp Makefile Tutorial

## GCC/Clang Compiler Steps

### Compilation (Assembling)

- Checks the C/C++ language syntax for error
- Generates object files
- Command: g++ main.cc -c
- Produces: main.o

### Linker

- Linking all the source files together, that is all the other object codes in the project.
- Generates the executable file
- Command: g++ main.o -o main
- Produces: main.out (.exe for Windows)

### Compiler Flags

- Debug: ```-g```
- Release: ```-O0 -O1 -O2 -O3 -Og```
- Includes: ```-I```
- Warnings: ```-Wall -Wextra -Wpedantic -Wconversion```

## Makefile Commands of the Template

### Makefile Variables

Convention is naming in upper snake_case.

```make
  VARIABLE_NAME = Value
```

Variables can be called by $(VARIABLE_NAME)

```make
  $(VARIABLE_NAME)
```

### Makefile Targets

Convention is naming in snake_case or camelCase.

```make
  targetName: Dependecies
    Command
```

Targets can be called by the ```make``` command.

```bash
  make targetName
```

### Makefile Phony Target

Sometimes you want your Makefile to run commands that do not represent files, for example the "clean" target. You may potentially have a file named clean in your main directory. In such a case Make will be confused because by default the clean target would be associated with this file and Make will only run it when the file doesn't appear to be up-to-date.

```make
.PHONY: clean
clean:
  rm -rf *.o
```

In terms of Make, a phony target is simply a target that is always out-of-date, so whenever you ask make <phony_target>, it will run, independent from the state of the file system.

### Build the Executable

Create the executable in either Debug or Release mode.

```bash
  make build DEBUG=0 # Build type is debug
  make build DEBUG=1 # Build type is release
```

### Run the Executable

Run the executable in either Debug or Release mode.

```bash
  make execute DEBUG=0 # Build type is debug
  make execute DEBUG=1 # Build type is release
```

### Variables of the Makefile Template

- Debug Mode: 1 (True) or 0 (False)
- ENABLE_WARNINGS: 1 (True) or 0 (False)
- WARNINGS_AS_ERRORS: 1 (True) or 0 (False)
- CPP_STANDARD: c++11, c++14, c++17, etc.

### Important Shortcuts of the Makefile Template

- ```$@```: the file name of the target
- ```$<```: the name of the first dependency
- ```$^```: the names of all dependencies
