# Quickmake
A makefile for creating C projects using the GCC compiler.

### Project Structure
This GNU makefile takes advantage of a specific directory structure. 
The following top-level directories are needed by this makefile to compile your code:

  - `src`: Contains the .c source files within different modules (also directories) specified in the makefile.
  - `test`: Contains the test .c source files within different test modules specified in the makefile.
  - `include`: Contains header .h files (not within modules) for the whole project. The compiler is set up to search for any headers included by the `#include "..."` preprocessor statement in this directory.

These directories are mandatory for the compilation process.

The compilation process creates a top-level `build` directory that containes the following subdirectories:

  - `bin`: Contains the executables created by the compilation process.
  - `modules`: Contains the modules with compiled object files for use in the main executable file.
  - `test_modules`: Contains the test modules with compiled object files for the project tests.
  
A `clean` target removes the entire `build` directory.

### How to get going

To get started we have to edited the following variables in the makefile to suit our project:

  - `MODULES` and `TEST_MODULES`: These variables reflect the modules containing source for the regular executable and the test executable. Both varibles are an whitespace-seperated list of directories names. 
  - `TEST_EXEC` and `EXEC`: These variables are the names of our executables we want to compile. As both executables will be placed in the same directory, it will be a smart idea to give different names to the variables.

Once this is done, use `make rollout` to create the directory structure discussed in the previous section.
Add in you code in the different modules.

To compile the tests use `make tests` (or `make run_tests` to compile and run them). 

To compile your regular (or release) executable simply type `make` and you should find your executable in the `build/bin/` directory.
