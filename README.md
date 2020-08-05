C++ Seed Project
================

This is a skeleton of a C++ project.
The project is using the following tools:

* Test Framework: Google Test
* Documentation Gen: Sphinx + Doxygen
* Build Tool: CMake
* Version Control System: Git
* Dependencies: CMake + [(Git & gitmodules)(*)]
* Coverage using gcov
* Exceptions with Stack Trace
* logging spdlog
* clang-format


# Make a new project

```
> pip install cookiecutter

> cookiecutter https://github.com/kiwi-lang/cxx_seed
You've downloaded /home/setepenre/.cookiecutters/cxx_seed before. Is it okay to delete and re-download it? [yes]: yes
project_name [test_project]: MyProject
author [Anonymous]: MyAuthor
project_brief [Testing]: My Awesome Project Description
copyright [Testing]: 2020 MyAuthor
> cd MyProject
> mkdir build
> cmake ..
> make -j 8
```


# Configuration

## Directory Layout

    src/          : Project Code source
    cmake/        : CMake Scripts (FindXXX.cmake)
    tests/        : Tests files (***_test.cpp)
    benchmark/    : Performance tests (***_bench.cpp)
    docs/         : Doxygen or Sphinx files
    data/         : Data (Used to store Performance results for each git commit)
    build/        : git ignored folder in which you can build the project
    dependencies/ : Dependencies source code

# Examples

Show casing logging, versioning and stack trace dump

    mkdir build
    cd build
    CC=gcc CXX=g++ cmake ..
    make
    ./bin/main
    [I] [09-02-2020 16:49:13.014] [28158] /home/setepenre/work/cxx_skeleton/src/main.cpp:16 main - Testing throwing function
    [I] [09-02-2020 16:49:13.014] [28158] /home/setepenre/work/cxx_skeleton/src/main.cpp:17 main - version hash  : f0d8eafa92a859e5537594425bae588206374c78
    [I] [09-02-2020 16:49:13.014] [28158] /home/setepenre/work/cxx_skeleton/src/main.cpp:18 main - version date  : 2016-06-02 16:45:46 -0400
    [I] [09-02-2020 16:49:13.014] [28158] /home/setepenre/work/cxx_skeleton/src/main.cpp:19 main - version branch: master
    [C] [09-02-2020 16:49:13.015] [28158] ./bin/main(_ZN3sym13get_backtraceB5cxx11Em+0x79) [0x564278526419]
    [C] [09-02-2020 16:49:13.015] [28158] ./bin/main(_ZN3sym14show_backtraceEv+0x28) [0x564278526d48]
    [C] [09-02-2020 16:49:13.015] [28158] ./bin/main(_ZNK3sym9Exception4whatEv+0x9) [0x564278526de9]
    [C] [09-02-2020 16:49:13.015] [28158] ./bin/main(+0x310d8) [0x5642784fd0d8]
    [C] [09-02-2020 16:49:13.015] [28158] /lib/x86_64-linux-gnu/libc.so.6(__libc_start_main+0xe7) [0x7fda53cbdb97]
    [C] [09-02-2020 16:49:13.015] [28158] ./bin/main(_start+0x2a) [0x5642784fe67a]

# Commands

    make enable-examples    # enable compiling examples
    make disable-examples
    make enable-release     # enable release compilation
    make enable-debug       # enable debug compilation
    make enable-bench       # enable benchmarks compilations
    make enable-test        # enable compiling tests
    make enable-doc         # enable generating docs
    make coverage           # execute tests & compute coverage
    make sphinx             # build sphinx documentation
    make sphinx-serve       # serve the documentation locally

# How to

## Get Coverage

```
cd build
make enable-test
make test
make coverage
firefox ./coverage/index.html
```

## Create a library/executable

* create a new folder `src/new_library` or `src/new_exec`
* add `src/new_library/src/CMakeLists.txt`
* create library files (`.cpp` and `.h`)
* create new CMake variable with library's files

```
SET(MY_LIBRARY_SRC
    file1.h
    file1.cpp
    file2.h
    file2.cpp
)
```

* Add the compile command:

```
ADD_LIBRARY(my_library ${MY_LIBRARY_SRC})
```

Alternatively

```
ADD_EXECUTABLE(my_exec ${MY_LIBRARY_SRC})
TARGET_LINK_LIBRARIES(my_exec dependencies)
```

## Add test header

* create file `stuff_test.h`
* add `#include "stuff_test.h"` to the `**_test.cpp`

## Add test executable

* create a new file `my_newtest_test.cpp`
* add `TEST_MACRO(my_newtest)` at the end of the file `tests/CMakeLists.txt`

## Add Benchmark

* create a new file `my_newbench_bench.cpp`
* add `TEST_BENCH(my_newbench)` at the end of the file `benchmark/CMakeLists.txt`

## Add (to be compiled) Dependency

* open git bash
* go to the project repo
* `git submodule add url_to_dependency_repo dependencies/library_name`

Example: add `opencv` to the project

    git submodule add https://github.com/Itseez/opencv.git dependencies/opencv
    git submodule add https://github.com/google/googletest.git dependencies/gtest

## Remove submodule

    rm -rf ../.git/modules/dependencies/library_name
    vi .gitmodules
    # remove the modules


## Add pre-compiled Dependencies

* Add `FindXXX.cmake` script in the `cmake/` folder
* Add `FIND_PACKAGE(XXX)` in `./CMakeLists.txt`  (pre-compiled section)

```
FIND_PACKAGE(openCV)
```

# Requirement

* C++ Compiler
* Python (gtest)
* CMake
* Git
* [Sphinx, Doxygen] (*)

(*) Optional


TODO
====

* How to
* Valgrind utils/Sanitizer Command/debbuger

        # Need to find a better way
        # Hardcoded:
        SET(LEAK_FLAG " -Werror -fsanitize=leak ") # this for clang
        SET(CMAKE_CXX_FLAGS ${LEAK_FLAG})

        # would be nice to have something like:
        # for each target "xyz" we have a
            - make san/leak/xyz
            - make san/thread/xyz
            - make lld/xyz
            - make format
            - make tidy
        # need to check how much caching is done between each exec version

* Goole Perf ?
* Generated files

Bugs
====

* `gtest` does not compile with `MinGW`. This a` MinGW` bug. You need to deactivate
pthreads for it to work (`-Dgtest_disable_pthreads=ON`)

