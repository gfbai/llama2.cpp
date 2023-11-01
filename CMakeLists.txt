cmake_minimum_required(VERSION 3.16)
project(llama2)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/..)

include_directories(${CMAKE_SOURCE_DIR})

# Define an option for OpenMP support
option(USE_OPENMP "Use OpenMP for parallelism" OFF)

# If USE_OPENMP is turned on, find OpenMP
if(USE_OPENMP)
    find_package(OpenMP)
    if(OpenMP_CXX_FOUND)
        add_definitions(-DUSE_OPENMP)
    endif()
endif()

add_executable(run run.cpp)

# Link OpenMP if it was found and if USE_OPENMP is turned on
if(OpenMP_CXX_FOUND AND USE_OPENMP)
    target_link_libraries(run PUBLIC OpenMP::OpenMP_CXX)
endif()

