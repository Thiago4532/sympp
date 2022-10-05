cmake_minimum_required(VERSION 3.12)

# Determine if sympp is built as a subproject (using add_subdirectory)
# or if it is the master project.
if (NOT DEFINED SYMPP_MASTER_PROJECT)
  set(SYMPP_MASTER_PROJECT OFF)
  if (CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    set(SYMPP_MASTER_PROJECT ON)
  endif ()
endif ()

# Set the default CMAKE_BUILD_TYPE to Release.
if (SYMPP_MASTER_PROJECT AND NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "" FORCE)
endif ()

# Export compile_commands.json
if (SYMPP_MASTER_PROJECT AND NOT CMAKE_EXPORT_COMPILE_COMMANDS)
    set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
endif ()

project(sympp)

set(SYMPP_SOURCES
    src/aux.cpp
)

# Enable test target if sympp is the master project
option(SYMPP_TEST "Generate the test target." ${SYMPP_MASTER_PROJECT})

if (LIBTD_MASTER_PROJECT)
    message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
endif()

if (NOT CMAKE_RUNTIME_OUTPUT_DIRECTORY)
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
endif ()

add_library(sympp ${SYMPP_SOURCES})
target_include_directories(sympp PUBLIC include/)

# Enable C++17 and warnings
target_compile_features(sympp PUBLIC cxx_std_17)

if (SYMPP_TEST)
    add_subdirectory(test)
endif ()
