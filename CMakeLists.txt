cmake_minimum_required(VERSION 3.12)
project(SysCalc VERSION 1.1.7 LANGUAGES CXX Fortran)

# Set C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find required packages
find_package(PkgConfig REQUIRED)
pkg_check_modules(LHAPDF REQUIRED lhapdf)

# Include directories
include_directories(include)
include_directories(${LHAPDF_INCLUDE_DIRS})

# Add compiler definitions
add_definitions(-DDROP_CGAL)

# Set optimization flags
set(CMAKE_CXX_FLAGS_RELEASE "-O")
set(CMAKE_Fortran_FLAGS_RELEASE "-O")

# Source files
set(SOURCES
    src/SysCalc.cc
    src/tinyxml2.cpp
    src/alfas_functions.f
    src/sys_calc.cc
)

# Create the executable
add_executable(sys_calc ${SOURCES})

# Link libraries
target_link_libraries(sys_calc ${LHAPDF_LIBRARIES})

# Set output directory to match original Makefile behavior
set_target_properties(
    sys_calc
    PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}
)
