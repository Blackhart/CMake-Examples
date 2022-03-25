# CMake Examples

- [CMake Examples](#cmake-examples)
  - [How to configure a cmake project](#how-to-configure-a-cmake-project)
  - [How to list project variables](#how-to-list-project-variables)
  - [How to build a cmake project](#how-to-build-a-cmake-project)
  - [How to make a proper installation of a cmake project](#how-to-make-a-proper-installation-of-a-cmake-project)
  - [How to open a project](#how-to-open-a-project)
  - [How to make a branch control structure](#how-to-make-a-branch-control-structure)
  - [How to create / call a function](#how-to-create--call-a-function)
  - [How to set a cmake minimum version](#how-to-set-a-cmake-minimum-version)
  - [How to set a project name, version, description, URL and language](#how-to-set-a-project-name-version-description-url-and-language)
  - [How to create / set a variable](#how-to-create--set-a-variable)
  - [How to destroy / unset a variable](#how-to-destroy--unset-a-variable)
  - [How to reference a variable](#how-to-reference-a-variable)
  - [How to iterate over a list](#how-to-iterate-over-a-list)
  - [How to create an executable](#how-to-create-an-executable)
  - [How to create a library](#how-to-create-a-library)
  - [How to prevent building in the source directory](#how-to-prevent-building-in-the-source-directory)
  - [How to add a subdirectory](#how-to-add-a-subdirectory)
  - [How to add a new module path](#how-to-add-a-new-module-path)
  - [How to include a module](#how-to-include-a-module)
  - [How to create an object file](#how-to-create-an-object-file)
  - [How to link against an object file](#how-to-link-against-an-object-file)

## How to configure a cmake project

```shell
// Minimal
cmake -S mySourceTree -B myBuildTree

// With a custom generator
cmake -G myGenerator -S mySourceTree -B myBuildTree

// With some options
cmake -S mySourceTree -B myBuildTree -D MyOption=MyOptionValue
```

## How to list project variables

```shell
// List non advanced variables
cmake -S mySourceTree -B myBuildTree -L

// List advanced variables as well
cmake -S mySourceTree -B myBuildTree -LA

// List variables with help messages
cmake -S mySourceTree -B myBuildTree -LAH
```

## How to build a cmake project

```shell
// After configuring your cmake project
cmake --build myBuildTree

// With multithreading enabled
cmake --build myBuildTree --parallel myNumberOfJobs

// Building a specific target
cmake --build myBuildTree --target myTarget

// Building a specific configuration [Release, Debug, ..].
// CMake uses Debug as default one
cmake --build myBuildTree --config myConfig

// Building but cleanning the old build first
cmake --build myBuildTree --clean-first
```

## How to make a proper installation of a cmake project

```shell
// After building your cmake project
cmake --install myBuildTree

// Installing a specific configuration [Release, Debug, ..]
cmake --install myBuildTree --config myConfig

// Installing in a custom directory
cmake --install myBuildTree --prefix myInstallDirectory
```

## How to open a project

```shell
// After configuring your cmake project
// This command will automatically launch visual studio
cmake --open myBuildTree
```

## How to make a branch control structure

```cmake
# Conditional block

if (<condition>)
  <commands>
elseif (<condition>)
  <commands>
else ()
  <commands>
endif ()

# Logical operators

NOT <condition>
<condition> AND <condition>
<condition> OR <condition>
```

## How to create / call a function

```cmake
function (myFunctionName myFunctionArg0 myFunctionArg1)
  message ("${myFunctionArg0} ${myFunctionArg1}")
endfunction ()

myFunctionName("arg0" "arg1")
```

## How to set a cmake minimum version

```cmake
cmake_minimum_required(VERSION <minimum-version>)
```

## How to set a project name, version, description, URL and language

```cmake
# Project version, description, url and language are optional. 
# These informations will be used when you will package your app.

project(myProject \
        VERSION <your project version here in the form <major>[.<minor>[.<patch>[.<tweak>]]]> \
        DESCRIPTION <your project description here> \
        HOMEPAGE_URL <your project URL here> \
        LANGUAGES <your project language here. Must be one of C, CXX, CUDA, OBJC, OBJCXX, Fortran, HIP, ISPC, ASM>)
```

## How to create / set a variable

```cmake
# Setting a normal variable
set(myVariable myValue)

# Setting an environment variable
set(ENV{myVariable} myValue)

# Setting a cache variable
# type could be: BOOL, FILEPATH, PATH, STRING, INTERNAL
# Use FORCE to overwrite the value already present in the cahe
set(myVariable myValue CACHE <type> <docstring> [FORCE])
```

## How to destroy / unset a variable

```cmake
# Unset a normal variable
unset(myVariable)

# Unset an environment variable
unset(ENV{myVariable})

# Unset a cache variable
unset(myVariable CACHE)
```

## How to reference a variable

```cmake
# To retrieve the value of a variable, type 
${myVariable}
# To retrieve the value of an environment variable, type 
$ENV{myVariable}
# To retrieve the value of a cached variable, type 
$CACHE{myVariable}
```

## How to iterate over a list

```cmake
set(myList "a;list;of;five;elements")

foreach (myListElement IN LISTS myList)
  <commands>
```

## How to create an executable

```cmake
add_executable(myExecutable [<source> ...])
```

## How to create a library

```cmake
# Use STATIC to create a static library (.lib, .a)
# Use SHARED to create a shared library (.dll, .so)

add_library(myLibrary [STATIC | SHARED] [<source> ...])
```

## How to prevent building in the source directory

```cmake
if(PROJECT_SOURCE_DIR STREQUAL PROJECT_BINARY_DIR)
  message(FATAL_ERROR
    "\n"
    "In-source builds are not allowed.\n"
    "Instead, provide a path to build tree like so:\n"
    "cmake -B <destination>\n"
    "\n"
    "To remove files you accidentally created execute:\n"
    "rm -rf CMakeFiles CMakeCache.txt\n"
  )
endif()
```

## How to add a subdirectory

```cmake
add_subdirectory(mySubdirectory)
```

## How to add a new module path

```cmake
list(APPEND CMAKE_MODULE_PATH myModulePath)
```

## How to include a module

```cmake
include(myModule)
```

## How to create an object file

```cmake
# Object files are .obj files produced by the compiler
add_library(myObjectFile OBJECT [<source> ...])
```

## How to link against an object file

```cmake
# Because object files are generated when building your app, you have to use generator expression to link them against an executable or library
add_library(myLibrary $<TARGET_OBJECTS:myObjectFile>)
add_executable(myExecutable $<TARGET_OBJECTS:myObjectFile>)
# Here, no more generator expression used because your are linking against a target
target_link_libraries(myTarget myObjectFile)

# if you want to link an object file against a shared library, you have to add this line too
set_target_properties(myObjectFile PROPERTIES POSITION_INDEPENDENT_CODE 1)
```