# <Project Name> 
#

# ------------------------------------------------------------------------------------------------
# compiler

CC = gcc

# ------------------------------------------------------------------------------------------------
# console

RED = \033[1;31m
GREEN = \033[1;32m
BLUE = \033[1;34m
YELLOW = \033[1;33m
NC = \033[1;0m

# ------------------------------------------------------------------------------------------------
# configurations

# project dir
PROJECT_DIR = <project_name>
COMMON_DIR = $(PROJECT_DIR)/common
CORE_DIR = $(PROJECT_DIR)/core
MODELS_DIR = $(PROJECT_DIR)/models
TESTS_DIR = $(PROJECT_DIR)/tests
TESTS_CORE_DIR = $(TESTS_DIR)/core
TESTS_MODELS_DIR = $(TESTS_DIR)/models

# build dir
BUILD_DIR = _build
BUILD_COMMON_DIR = $(BUILD_DIR)/common
BUILD_CORE_DIR = $(BUILD_DIR)/core
BUILD_TESTS_CORE_DIR = $(BUILD_DIR)/tests/core
BUILD_TESTS_MODELS_DIR = $(BUILD_DIR)/tests/models
# libs
LIB_COMMON = lib<project>_<module>.a
LIB_CORE = lib<project_module>.a

# ------------------------------------------------------------------------------------------------

# common
COMMON_SOURCES := $(shell find $(COMMON_DIR) -name "*.c")
COMMON_HEADERS := $(shell find $(COMMON_DIR) -name "*.h")
COMMON_OBJECTS := $(patsubst %, $(BUILD_COMMON_DIR)/%, $(notdir $(COMMON_SOURCES:.c=.o)))

# core
CORE_SOURCES := $(wildcard $(CORE_DIR)/*.c)
CORE_HEADERS := $(wildcard $(CORE_DIR)/*.h)
CORE_OBJECTS := $(patsubst %, $(BUILD_CORE_DIR)/%, $(notdir $(CORE_SOURCES:.c=.o)))

# tests: core
TESTS_CORE_SOURCES := $(wildcard $(TESTS_DIR)/core/*.c)
TESTS_CORE_OBJECTS := $(patsubst %, $(BUILD_TESTS_CORE_DIR)/%, $(notdir $(TESTS_CORE_SOURCES:.c=.o)))
TESTS_CORE_TARGETS := $(patsubst %, $(BUILD_TESTS_CORE_DIR)/%, $(notdir $(TESTS_CORE_OBJECTS:.o=)))
# tests: models
TESTS_MODELS_SOURCES := $(wildcard $(TESTS_DIR)/models/*.c)

# ------------------------------------------------------------------------------------------------
# common

COMMON_CFLAGS = -Wall -I$(COMMON_DIR)


$(BUILD_COMMON_DIR)/$(LIB_COMMON) : $(COMMON_OBJECTS)
	@echo "$(RED)Linking $@ $(NC)"
	$(AR) cr $@ $^
	@echo "$(RED)$(LIB_COMMON) is saved at $(BUILD_COMMON_DIR)/$(LIB_COMMON) $(NC)"
	# @$(AR) -t $(BUILD_COMMON_DIR)/$(LIB_COMMON)

$(BUILD_COMMON_DIR)/%.o : $(COMMON_DIR)/%.c
	@echo "$(RED)Compiling $< $(NC)"
	$(CC) $(COMMON_CFLAGS) -c $< -o $@

# ------------------------------------------------------------------------------------------------
# core

CORE_CFLAGS = -Wall -I$(CORE_DIR) -I$(COMMON_DIR)

$(BUILD_CORE_DIR)/$(LIB_CORE) : $(CORE_OBJECTS) $(COMMON_OBJECTS)
	@echo "$(RED)Linking $@ $(NC)"
	$(AR) cr $@ $^ 
	@echo "$(RED)$(LIB_CORE) is saved at $(BUILD_CORE_DIR)/$(LIB_CORE)$(NC)"
	@$(AR) -t $(BUILD_CORE_DIR)/$(LIB_CORE)

$(BUILD_CORE_DIR)/%.o : $(CORE_DIR)/%.c 
	@echo "$(RED)Compiling $< $(NC)"
	$(CC) $(CORE_CFLAGS) -c $< -o $@ 


# ------------------------------------------------------------------------------------------------
# tests

TESTS_CFLAGS = -Wall -I$(COMMON_DIR) -I$(CORE_DIR)
TESTS_LFLAGS = -L$(BUILD_CORE_DIR) -l<project>_<module> -lm 

# tests_core
tests_core: $(TESTS_CORE_TARGETS)
	

$(BUILD_TESTS_CORE_DIR)/% : $(BUILD_TESTS_CORE_DIR)/%.o
	@echo "$(RED)Linking $@ $(NC)"
	$(CC) -o $@ $^ $(TESTS_LFLAGS)

$(BUILD_TESTS_CORE_DIR)/%.o : $(TESTS_CORE_DIR)/%.c
	@echo "$(RED)Compiling $< $(NC)"
	$(CC) -c $< -o $@ $(TESTS_CFLAGS)


# ------------------------------------------------------------------------------------------------
# make

.PHONY: help all run build clean

help:
	@echo "$(GREEN)Command List:$(NC)"
	@echo "$(GREEN)- all$(NC) building all"
	@echo "$(GREEN)- libs$(NC) ()"
	@echo "$(GREEN)- tests$(NC) ()"
	@echo "$(GREEN)- run$(NC) ()"
	@echo "$(GREEN)- build$(NC) ()"
	@echo "$(GREEN)- clean$(NC) ()"

all: clean build libs tests 

run: all

libs: $(BUILD_COMMON_DIR)/$(LIB_COMMON) $(BUILD_CORE_DIR)/$(LIB_CORE)

tests: tests_core

run_tests: tests
	@echo "$(RED)run tests_core:$(NC)"
	$(foreach test, $(TESTS_CORE_TARGETS), $(test);)

build:
	mkdir -p $(BUILD_DIR)
	mkdir -p $(BUILD_COMMON_DIR)
	mkdir -p $(BUILD_CORE_DIR)
	mkdir -p $(BUILD_TESTS_CORE_DIR)
	mkdir -p $(BUILD_TESTS_MODELS_DIR)

clean:
	rm -r $(BUILD_DIR)

