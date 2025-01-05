DEBUG ?= 0
ENABLE_WARNINGS ?= 1
WARNING_AS_ERRORS ?= 0


CXX_STANDARD = c++17


ifeq ($(ENABLE_WARNINGS), 1)
CXX_WARNINGS = -Wall -Wextra -Wpedantic
else
CXX_WARNINGS = 
endif

ifeq ($(WARNING_AS_ERRORS), 1)
CXX_WARNINGS += -Werror
endif


CXX = g++
CXXFLAGS = $(CXX_WARNINGS) -std=$(CXX_STANDARD)

CPPFLAGS = -I $(INCLUDE_DIR)
LDFLAGS =

ifeq ($(DEBUG), 1)
CXXFLAGS += -g -O0
EXECUTABLE_NAME = mainDebug
else
CXXFLAGS += -O3
EXECUTABLE_NAME = mainRelease
endif

CXX_COMPILER_CALL = $(CXX) $(CXXFLAGS) $(CPPFLAGS)

CXX_SOURCES = $(wildcard $(SOURCE_DIR)/*.cc)

CXX_OBJECTS = $(patsubst $(SOURCE_DIR)/%.cc, $(BUILD_DIR)/%.o, $(CXX_SOURCES))


INCLUDE_DIR = include
SOURCE_DIR = src
BUILD_DIR = build

all: create build execute

create: 
	@mkdir -p $(BUILD_DIR)

build: create $(CXX_OBJECTS)
	$(CXX_COMPILER_CALL) $(CXX_OBJECTS) $(LDFLAGS) -o $(BUILD_DIR)/$(EXECUTABLE_NAME)

execute:
	./$(BUILD_DIR)/$(EXECUTABLE_NAME)

clean:
	rm -rf $(BUILD_DIR)


$(BUILD_DIR)/%.o: $(SOURCE_DIR)/%.cc
	$(CXX_COMPILER_CALL) -c $< -o $@


.PHONY: create build execute clean