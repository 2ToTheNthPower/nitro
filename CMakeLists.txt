cmake_minimum_required(VERSION 3.15)
project(nitro)

# Set C++ standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Build RocketSim library
add_subdirectory(RocketSim)

# Find Python and pybind11
find_package(Python COMPONENTS Interpreter Development.Module REQUIRED)
find_package(pybind11 CONFIG REQUIRED)

# Create Python extension module
pybind11_add_module(nitro src/bindings.cpp)
target_link_libraries(nitro PRIVATE RocketSim)

# Include directories - only include RocketSim/src
# Bullet headers are included via relative paths from RocketSim headers
target_include_directories(nitro PRIVATE 
    ${CMAKE_CURRENT_SOURCE_DIR}/RocketSim/src
)

# Install the module and type stubs
install(TARGETS nitro DESTINATION .)
install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/src/nitro.pyi DESTINATION .)
