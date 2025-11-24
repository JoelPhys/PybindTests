cmake_minimum_required(VERSION 3.15)
project(superfastcode LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

# Include Eigen headers
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# Find Python and pybind11
find_package(Python3 COMPONENTS Interpreter Development REQUIRED)
find_package(pybind11 REQUIRED)

# Your source files
add_library(superfastcode MODULE
    ray_intersection.cpp
    render.cpp
    test1.cpp
)

# Link against pybind11 and Python
target_link_libraries(superfastcode
    PRIVATE
        pybind11::module
        Python3::Python
)

# Ensure the output is a proper Python extension (no "lib" prefix)
set_target_properties(superfastcode PROPERTIES
    PREFIX ""         # produces superfastcode.so instead of libsuperfastcode.so
    SUFFIX ".so"      # ensure correct extension for Python modules
)

