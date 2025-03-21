# Minimum CMake version required
cmake_minimum_required(VERSION 3.10)

# Project name
project(MyProject VERSION 1.0 LANGUAGES CXX)

# Allow user to specify the compiler
set(CMAKE_CXX_COMPILER "g++" CACHE STRING "C++ compiler")

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Define build types
set(CMAKE_BUILD_TYPE "Debug" CACHE STRING "Choose the type of build: Debug Develop Release")
set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Develop" "Release")

# Source and header files
file(GLOB SOURCES "src/*.cpp")
file(GLOB HEADERS "include/*.h" "include/*.hpp")

# Include directories
include_directories(include)

# Create executable
add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS})

# Use native architecture
target_compile_options(${PROJECT_NAME} PRIVATE -march=native)

# Build type specific configurations
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    target_compile_options(${PROJECT_NAME} PRIVATE
            -g                  # Debug symbols
                    -O0                 # No optimization
                            -Wall               # Basic warnings
                                    -Wextra             # Extra warnings
                                            -Wpedantic          # Strict ISO C++
                                                    -Wshadow            # Warn about variable shadowing
                                                            -Wnon-virtual-dtor  # Warn about non-virtual destructors
                                                                    -Wcast-align        # Warn about potential alignment issues
                                                                            -Wunused            # Warn about unused variables
                                                                                )
                                                                                elseif(CMAKE_BUILD_TYPE STREQUAL "Develop")
                                                                                    target_compile_options(${PROJECT_NAME} PRIVATE
                                                                                            -g                  # Debug symbols
                                                                                                    -O1                 # Minimal optimization
                                                                                                            -Wall               # Basic warnings
                                                                                                                    -Wextra             # Extra warnings
                                                                                                                            -Wpedantic          # Strict ISO C++
                                                                                                                                )
                                                                                                                                elseif(CMAKE_BUILD_TYPE STREQUAL "Release")
                                                                                                                                    target_compile_options(${PROJECT_NAME} PRIVATE
                                                                                                                                            -O3                 # Full optimization
                                                                                                                                                    -DNDEBUG            # Disable assertions
                                                                                                                                                            -Wall               # Basic warnings
                                                                                                                                                                )
                                                                                                                                                                endif()

                                                                                                                                                                # Print build configuration
                                                                                                                                                                message(STATUS "Build type: ${CMAKE_BUILD_TYPE}")
                                                                                                                                                                message(STATUS "C++ Compiler: ${CMAKE_CXX_COMPILER}")