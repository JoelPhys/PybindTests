cmake_minimum_required(VERSION 3.15)
project(superfastcode LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

# Include Eigen headers
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# Optimization flags
set(CMAKE_BUILD_TYPE Release)

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

# Ensure the output is a proper Python extension
if(WIN32)
    # Windows needs .pyd
    set_target_properties(superfastcode PROPERTIES
        PREFIX ""         # removes "lib" prefix
        SUFFIX ".pyd"
    )
else()
    # Linux/macOS
    set_target_properties(superfastcode PROPERTIES
        PREFIX ""         # removes "lib" prefix
        SUFFIX ".so"
    )
endif()
