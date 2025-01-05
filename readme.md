## https://stackoverflow.com/questions/24777289/what-is-in-makefile




'''

?= indicates to set the KDIR variable only if it's not set/doesn't have a value.

For example:

KDIR ?= "foo"
KDIR ?= "bar"

test:
    echo $(KDIR)

'''