# Quickmake
A makefile for creating C projects using the GCC compiler.

### Project Structure
This GNU makefile takes advantage of a specific directory structure. 
The following top-level directories are needed by this makefile to compile your code:

  - `src`: Contains the .c source files within different modules.
  - `test`: Contains the test .c source files within different test modules.
  - `include`: Contains header .h files (not within modules) for the whole project. The compiler is set up to search for any headers included by the `#include "..."` preprocessor statement in this directory.

These directories are mandatory for the compilation process.

The compilation process creates a top-level `build` directory that containes the following subdirectories:

  - `bin`: Contains the executables created by the compilation process.
  - `modules`: Contains the modules with compiled object files for use in the main executable file.
  - `test_modules`: Contains the test modules with compiled object files for the project tests.
  
A `clean` target removes the entire `build` directory. 

Customization of the directories listed above can be done the `buildvar.mk` file stored with the makefile.

### How to get going

To get started you have to edited the following variables in the `buildvar.mk` file to suit your project:

  - `MODULES` and `TEST_MODULES`: These variables reflect the modules containing source for the regular executable and the test executable. Both varibles are an whitespace-seperated list of directories names with no trailing slash. 
  - `TEST_EXEC` and `EXEC`: These variables are the names of our executables we want to compile. As both executables will be placed in the same directory it will be a good idea to give different names to the variables.

Once this is done, use `make rollout` to create the directory structure discussed in the previous section.
Add in you code in the different modules.

To compile the tests use `make tests` (or `make run_tests` to compile and run them). 

To compile your regular (or release) executable simply type `make` and you should find your executable in the `build/bin/` directory.
