cmake_minimum_required(VERSION 3.12)
project(SysCalc VERSION 1.1.7 LANGUAGES CXX Fortran)

# Set C++ standard
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find required packages
find_package(PkgConfig REQUIRED)
pkg_check_modules(LHAPDF REQUIRED lhapdf)

find_package(tinyxml2 CONFIG REQUIRED)

# Set optimization flags
set(CMAKE_CXX_FLAGS_RELEASE "-O")
set(CMAKE_Fortran_FLAGS_RELEASE "-O")

# Create the executable
add_executable(sys_calc src/SysCalc.cc src/alfas_functions.f src/sys_calc.cc)

target_include_directories(sys_calc PRIVATE include)
target_include_directories(sys_calc PRIVATE ${LHAPDF_INCLUDE_DIRS})

# Add compiler definitions
target_compile_definitions(sys_calc PRIVATE -DDROP_CGAL)

# Link libraries
target_link_directories(sys_calc PRIVATE ${LHAPDF_LIBRARY_DIRS})
target_link_libraries(sys_calc PRIVATE ${LHAPDF_LIBRARIES} tinyxml2::tinyxml2)

# Set output directory to match original Makefile behavior
set_target_properties(
    sys_calc
    PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}
)

install(TARGETS sys_calc DESTINATION bin)
