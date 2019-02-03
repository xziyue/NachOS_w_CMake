project(NachOS)
enable_language(CXX ASM)
cmake_minimum_required(VERSION 3.0)

# this is just an effort to provide IDE support for NachOS
# being able to compile NachOS with this cmake configuration
# DOES NOT NECESSARILY MEANs THAT IT COMPILES WITH THE ORIGINAL MAKEFILE!
# please double check if it is compatible with the original settings

# SOME MORE HINTS:

# the build directories will be different, make sure to clean them up before submitting

# USE AT YOUR OWN RISK!


# define some system-dependent macros
# to correspond with the macros in ~/lib/sysdep.cc

message(STATUS "your processor is ${CMAKE_SYSTEM_PROCESSOR}")
if(NOT (${CMAKE_SYSTEM_PROCESSOR} MATCHES "(x86)|(X86)|(amd64)|(AMD64)"))
    message(SEND_ERROR "only x86 architecture is supported in this config!")
else()
    add_definitions(-Dx86)
endif()

set(KNOWN_OS FALSE)
# whether to build in 64-bit mode
# for apple, this is true because 32-bit architecture is deprecated
set(NACHOS_64 FALSE)

if(APPLE)
    message(STATUS "osx detected")
    add_definitions(-DBSD)
    set(KNOWN_OS TRUE)
    set(NACHOS_64 TRUE)
else()
    if(UNIX)
        message(STATUS "unix detected")
        add_definitions(-DLINUX)
        set(KNOWN_OS TRUE)
    endif()
endif()

if(NOT KNOWN_OS)
    message(SEND_ERROR "An unknown operating system encountered!")
endif()

if(NOT NACHOS_64)
    message(STATUS "building 32-bit NachOS")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m32")
else()
    message(STATUS "building 64-bit NachOS")
endif()

# make sure to always compile in debug mode without any optimization
set(CMAKE_BUILD_TYPE DEBUG)

# I think these two flags should be already set by cmake
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -m32 -g -Wall ")

# just to conform with the make rule of switch.S
set(CMAKE_ASM_FLAGS "${CMAKE_ASM_FLAGS} -P -m32")

# just to make life easier
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-write-strings -Wno-char-subscripts")

# using the oldest C++ standard possible to emulte the compilers on the remote machine
set (CMAKE_CXX_STANDARD 98)

# these are the defitions that can be changed as we perfect our NachOS
add_definitions(-DFILESYS_STUB -DRDATA -DSIM_FIX)

# I am not sure what this definition is for
add_definitions(-DCHANGED)

# these are the include directories of NachOS
include_directories(${PROJECT_SOURCE_DIR}/filesys
        ${PROJECT_SOURCE_DIR}/lib
        ${PROJECT_SOURCE_DIR}/machine
        ${PROJECT_SOURCE_DIR}/network
        ${PROJECT_SOURCE_DIR}/test
        ${PROJECT_SOURCE_DIR}/threads
        ${PROJECT_SOURCE_DIR}/userprog)


# I am using file commands intensively, so if one wants to add some new files to the project, make sure to reload cmake
file(GLOB LIB_INC ${PROJECT_SOURCE_DIR}/lib/*.h)
file(GLOB LIB_SRC ${PROJECT_SOURCE_DIR}/lib/*.cc)
# excluding templates, which has been already included in the headers
list(FILTER LIB_SRC EXCLUDE REGEX ".*hash.cc$")
list(FILTER LIB_SRC EXCLUDE REGEX ".*list.cc$")


file(GLOB MACHINE_INC ${PROJECT_SOURCE_DIR}/machine/*.h)
file(GLOB MACHINE_SRC ${PROJECT_SOURCE_DIR}/machine/*.cc)

file(GLOB THREAD_INC ${PROJECT_SOURCE_DIR}/threads/*.h)
file(GLOB THREAD_SRC ${PROJECT_SOURCE_DIR}/threads/*.cc)
# excluding main.cc
list(FILTER THREAD_SRC EXCLUDE REGEX ".*main.cc$")
# excluding templates
list(FILTER THREAD_SRC EXCLUDE REGEX ".*synchlist.cc$")


file(GLOB USERPROG_INC ${PROJECT_SOURCE_DIR}/userprog/*.h)
file(GLOB USERPROG_SRC ${PROJECT_SOURCE_DIR}/userprog/*.cc)

file(GLOB FILESYS_INC ${PROJECT_SOURCE_DIR}/filesys/*.h)
file(GLOB FILESYS_INC ${PROJECT_SOURCE_DIR}/filesys/*.cc)

file(GLOB NETWORK_INC ${PROJECT_SOURCE_DIR}/network/*.h)
file(GLOB NETWORK_SRC ${PROJECT_SOURCE_DIR}/network/*.cc)

# the special assembly file switch.s
set(ASM_SWITCH ${PROJECT_SOURCE_DIR}/threads/switch.S)

set(NACHOS_HEADERS ${LIB_INC} ${MACHINE_INC} ${THREAD_INC} ${USERPROG_INC} ${FILESYS_INC} ${NETWORK_INC})

add_library(NACHOS_DEPEND ${NACHOS_HEADERS} ${LIB_SRC}  ${MACHINE_SRC}
         ${THREAD_SRC}  ${USERPROG_SRC}
         ${FILESYS_SRC}  ${NETWORK_SRC}
        ${ASM_SWITCH}
        )


set(NACHOS_ENTRANCE_SRC ${PROJECT_SOURCE_DIR}/threads/main.cc)

# the entrance file
add_executable(NACHOS_ENTRANCE ${NACHOS_HEADERS} ${NACHOS_ENTRANCE_SRC})
target_link_libraries(NACHOS_ENTRANCE NACHOS_DEPEND)


macro(print_all_variables)
    message(STATUS "print_all_variables------------------------------------------{")
    get_cmake_property(_variableNames VARIABLES)
    foreach (_variableName ${_variableNames})
        message(STATUS "${_variableName}=${${_variableName}}")
    endforeach()
    message(STATUS "print_all_variables------------------------------------------}")
endmacro()