########
#
# Makefile that finds C++-source files and generates compilation rules for
# use with the G++ compiler.
#
# This makefile relies a specific source tree organization created by the user:
#	- SOURCE_PREFIX: this director contains the different MODULES directories which in turn 
# contains the .cc/.cpp source files for the project
#	- TEST_PREFIX: contains the TEST_MODULES directories which contains the .cc/.cpp source files for 
# the project's tests
#	- INCLUDE_PREFIX: contains headers for the projects 
# 
# Using this source tree organization the following phony targets are availiable:
# 	- all: Default target. Compiles code in SOURCE_PREFIX and creates the EXEC exectutable in
# the BUILD_PREFIX folder under the "bin/" folder
#	- tests: Compiles the test code and creates the TEST_EXEC executable in
# the BUILD_PREFIX folder under the "bin/" folder
#	- run_tests: Relies on the "tests" target and then executes the TEST_EXEC
#	- rollout: Creates the source tree organization
#	- clean: Removes the BUILD_PREFIX directory
#	- testcheckdirs: Creates the directory tree for the compilation output from compiling the tests
#	- checkdirs: Creates the directory tree for the compilation output from compiling the EXEC executable
#
########
CXX = g++
CXXFLAGS = -g -std=c++11 -Wall -Wextra -Wpedantic
LDFLAGS =
LDLIBS = -lsfml-graphics -lsfml-window -lsfml-system

## EDIT THESE VARIABLES TO CUSTOMIZE THE BUILD  

# Modules within source directory and test directory
MODULES = fun
TEST_MODULES = my_test_module

# test executable name 
TEST_EXEC = test

# default executable
EXEC = main_sfml

# directory names
BUILD_PREFIX = build/
SOURCE_PREFIX = src/
TEST_PREFIX = tests/
INCLUDE_PREFIX = include/

### END OF EDIT THIS

BIN_PATH = $(addprefix $(BUILD_PREFIX), bin/)

EXEC_PATH = $(addprefix $(BIN_PATH), $(EXEC))
SRC_DIR = $(addprefix $(SOURCE_PREFIX), $(MODULES))
BUILD_DIR = $(addprefix $(BUILD_PREFIX), $(addprefix modules/, $(MODULES)))

SRC = $(foreach sdir, $(SRC_DIR), $(wildcard $(sdir)/*.cc)) $(foreach sdir, $(SRC_DIR), $(wildcard $(sdir)/*.cpp)) 
OBJ = $(patsubst $(SOURCE_PREFIX)%.cc, $(BUILD_PREFIX)modules/%.o, $(filter %.cc, $(SRC))) $(patsubst $(SOURCE_PREFIX)%.cpp, $(BUILD_PREFIX)modules/%.o, $(filter %.cpp, $(SRC)))

TEXEC_PATH = $(addprefix $(BIN_PATH), $(TEST_EXEC))
TEST_SRC_DIR = $(addprefix $(TEST_PREFIX), $(TEST_MODULES))
TEST_DIR = $(addprefix $(BUILD_PREFIX), $(addprefix test_modules/, $(TEST_MODULES)))

T_SRC = $(foreach tdir, $(TEST_SRC_DIR), $(wildcard $(tdir)/*.cc)) $(foreach tdir, $(TEST_SRC_DIR), $(wildcard $(tdir)/*.cpp))
T_OBJ = $(patsubst $(TEST_PREFIX)%.cc, $(BUILD_PREFIX)test_modules/%.o, $(filter %.cc, $(T_SRC))) $(patsubst $(TEST_PREFIX)%.cpp, $(BUILD_PREFIX)test_modules/%.o, $(filter %.cpp, $(T_SRC)))

VPATH = $(SRC_DIR) $(TEST_SRC_DIR)

.PHONY: all test checkdirs testcheckdirs clean run_tests rollout

all: checkdirs $(EXEC_PATH)

tests: testcheckdirs $(TEXEC_PATH)

run_tests: tests
	exec $(TEXEC_PATH)

rollout: $(INCLUDE_PREFIX) $(SRC_DIR) $(TEST_SRC_DIR)

testcheckdirs: checkdirs $(TEST_DIR)

checkdirs: $(BUILD_DIR) $(BIN_PATH)
	
clean:
	rm -rf $(BUILD_PREFIX)

$(EXEC_PATH): $(OBJ)
	$(CXX) -iquote$(INCLUDE_PREFIX) $(CFLAGS) $^ -o $@ $(LDFLAGS) $(LDLIBS)

$(TEXEC_PATH): $(OBJ) $(T_OBJ)
	$(CXX) -iquote$(INCLUDE_PREFIX) $(CFLAGS) $^ -o $@ $(LDFLAGS) $(LDLIBS)

## Auto-generating implicit rules

# Stuff in define gets evaled twice so we need double $s

GEN_DIRS = $(BIN_PATH) $(BUILD_DIR) $(TEST_DIR) $(SRC_DIR) $(TEST_SRC_DIR) $(INCLUDE_PREFIX)
GEN_C_RULES = $(BUILD_DIR) $(TEST_DIR)

define make-c-goal 
$1/%.o: %.cc 
	$(CXX) -iquote$(INCLUDE_PREFIX) $(CFLAGS) -c $$< -o $$@ $(LDFLAGS) $(LDLIBS)
$1/%.o: %.cpp 
	$(CXX) -iquote$(INCLUDE_PREFIX) $(CFLAGS) -c $$< -o $$@ $(LDFLAGS) $(LDLIBS)
endef

define make-dirs
$1:
	mkdir -p $$@
endef

# the following line creates all the implicit rules for the subdirectories
$(foreach bdir, $(GEN_C_RULES), $(eval $(call make-c-goal, $(bdir))))
$(foreach dir, $(GEN_DIRS), $(eval $(call make-dirs, $(dir))))