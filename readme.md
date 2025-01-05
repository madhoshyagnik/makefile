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
