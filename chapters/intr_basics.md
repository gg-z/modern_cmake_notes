# Introduction to the basics   

## 000. Introduction and Minimum Example  

## Minimum Version  
```cmake
# for function name, case is insensitive 
cmake_minimum_required(VERSION 3.1)

# VERSION val  # variable name and variable value 
cmake_minimum_required(VERSION 3.1...3.15)

if(${CMAKE_VERSION} VERSION_LESS 3.12)
    cmake_policy(VERSION ${CMAKE_MAJOI_VERSION}.${CMAKE_MINOR_VERSION})
else()
    cmake_policy(VERSION 3.15)
endif()  
```


## Setting a project   

```cmake
# strings are quoted, whitespace doesn't matter, and the name of the projcet is the first argument(positional)  
project(Myproject VERSION 1.0 
                  DESCRIPTION "Very nice project"
                  LANGUAGE CXX
        )

```


## Making an executable  
```cmake
# 'one' is both the name of the executable file generated and the name of the cmake target created.  The source file list comes next, and you can list as many as you'd like.   Cmake is smart, and will only compile source file extensions.  The headers will be , for most intents and purposes, ignored; this only reason to list them is to get them to show up in IDEs.  Targets show up as folders in many IDEs. 
add_executable(one two.cpp three.cpp)
```   

## Making a library   
```cmake
# you get to pick a type of library, STATIC, SHARED, or MODULE. if you leave this choice off, the value of BUILD_SHARED_LIBS will be used to pick between STATIC and SHARED.  

# in following sections, often you'll need to make a fictional target, that is one where nothing needs to be compiled, for example, for a header-only library, That is called an INTERFACE library, and is another choice. The only difference is it cannot be followed by filenames. 
add_library(one STATIC two.cpp three.h)  
```


## Targes are your friend   

```cmake 
# add informatin about target  

# targe_include_directories adds an include directory to a target. PUBLIC does not mean much for an executable; for a library it lets CMake know that any targets that link to this target must also need that include directory.  Other options are PRIVATE (only affect the current target, not dependencies), and INTERFACE. (only needed for dependencies)
target_include_directories(one PUBLIC include)  

# we can the chain targets:  
add_library(abother STATIC another.cpp another.h)
target_link_libraries(another PUBLIC one)   

# target_link_libraries is probably the most useful and confusing command in CMake. 
# 1. It takes a target (another) and adds a dependency iff a target is given. 
# 2. If no target of the name (one) exists, then it adds a link to a library called one on you path. 
# 3. You can give it a full path to a library. Or a linker flag. 
```  

## Dive in  

```cmake
# focusing on using targets everywhere, and you'll be fine.  
# Targets can have include directories, linked libraries(or linked targets), compile options, compile definitions, compile features, and more.  
```

```cmake 
# example  
cmake_minimum_version(VERSIOn 3.8)

project(Calculator LANGUAGE CXX)  

add_library(calclib STATIC src/calib.cpp include/calc/lib.hpp)  
target_include_directories(calclib PUBLIC include)  
target_compile_features(calclib PUBLIC cxx_std_11)  

add_executable(calc app/calc.cpp)    
target_link_libraries(calc PUBLIC calclib)     
```

## 001 Variables and the Cache  


## Variable 
```cmake 
# a local variable, the names of variables are usually all caps, and the value follows.  You access a variable by using{} such as ${MY_VARIABLE}. 
set(MY_VARIABLE 'value')  

# list  
set(MY_LIST "a" "b") 
set(MY_LIST "a;b")  
# the list( command has utilities for working with lists, and separate_arguments will turn a space separated string into ad list(inplace). Note that an unquoted value in CMake is the same as a quoted one if there are no spaces in it; this allows you to skip the quotes most of the time when working with value that you know could not contain spaces.  

# When a varibale is expanded using ${} syntax, all the same rules about sapce apply. Be especially careful with paths; paths may contain a space at any time and should always be quoted when they are a varibale(nerver wirte ${MY_PATH}, always should be "${MY_PATH}")  


```

## Cache Variable  
```cmake 
# If you want to set a variable from the command line, CMake offers a variable cache. Some variables are already here, like CMAKE_BUILD_TYPE. The syntax for declaring a variable and setting it if it is not already set is
# this will not replace an existing value.  
set(MY_CACHE_VARIABLE "VALUE" CACHE STRING "Description")  

# If you want to use these variables as a make-shift global variable, then you can do : 
set(MY_VARIABLE "VALUE" CACHE STRING "" FORCE) 
set(MY_VARIABLE "VALUE" CACHE INTERNAL "")
mark_as_advaned(MY_CACHE_VARIABLE)  

# since BOOL is such a common variable type, you can set it  more succinctly with the shortcut:
option(MY_OPTION "This is settable from the command line" OFF)  
```

## Environmetn variables  
```cmake 
set(ENV{varibale_name} value) 
$ENV{varibale_name}  
```

## The Cache    
The cache is actually just a text file, CMakeCache.txt, that gets created in the build directory when you run CMake. This is how CMake remembers anything you set, so you don't have to re-list your options every time you rerun CMake.   


## Properties    
```cmake 
# property is like a variable, but it is attached to some other item, like a directory or a target. A global property can be a useful uncached global variable. Many target properties are initialized from a matching variable with CMAKE_ at the front. 
set_property(TARGET TargetName PROPERTY CXX_STANDARD 11)
set_target_properties(TargetName PROPERTIES CXX_STANDARD 11)  

# get property 
get_property(ResutlVariable TARGET TargetName PROPERTY CXX_STANDARD) 
```


## 002  Programming in CMake  

## Control flow  
```cmake 
if(variable)
    # if variable is 'ON', 'YES', 'TRUE', 'Y', or non zero number

else()
    # if variable is '0', 'OFF', 'NO', 'FALSE', 'N', 'IGNORE', 'NOTFOUND', '""', or ends in '-NOTFOUND'

endif()

# the following code is okey 
if("${variable}") 

```



## Generator-expressions  
```
```

## Macros and Functions  
```cmake
function(SIMPLE REQUIRED_ARG)
    message(STATUS "Simple arguments: ${REQUIRED_ARG}, followed by ${ARGV}")
    set(${REQUIRED_ARG} "From SIMPLE" PARENT_SCOPE) 
endfunction()

simple(This)
message("Output: ${This}") 
```

## Arguments 
```cmake
function(COMPLEX)
    cmake_parse_arguments(
        COMPLEX_PREFIX
        "SINGMAL;ANOTHER"
        "ONE_VALUE;ALSO_ONE_VALUE"
        "MULTI_VALUES"
        ${ARGN}
    )

endfunction()

complex(SINGAL ONE_VALUE value MULTI_VALUES some other values)  

# inside the function after this call, you'll find:  
COMPLEX_PREFIX_SIGNAL = TRUE
COMPLEX_PREFIX_ANOTHER = FALSE
COMPLEX_PREFIX_ONE_VALUE = "value"
COMPLEX_PREFIX_ALSO_ONE_VALUE = <UNDEFINED>
COMPLEX_PREFIX_MULTI_VALUES = "some;other;values"  
```



## How to structure your project.   
```cmake 
- project
  - .gitignore
  - README.md
  - LICENCE.md
  - CMakeLists.txt
  - cmake
    - FindSomeLib.cmake
    - something_else.cmake
  - include
    - project
      - lib.hpp
  - src
    - CMakeLists.txt
    - lib.cpp
  - apps
    - CMakeLists.txt
    - app.cpp
  - tests
    - CMakeLists.txt
    - testlib.cpp
  - docs
    - CMakeLists.txt
  - extern
    - googletest
  - scripts
    - helper.py
```

```cmake
add_subdirectory to add a subdirectory containing a CMakeLists.txt 
```





















