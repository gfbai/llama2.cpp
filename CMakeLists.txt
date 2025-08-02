cmake_minimum_required(VERSION 3.16)
project(llama2)

set(CMAKE_CXX_STANDARD 17)

# Define an option for OpenMP support
option(USE_OPENMP "Use OpenMP for parallelism" OFF)  # Default is not using OpenMP
option(USE_OPT_ARGS_ONLY "Use build arguments for optimization (with OpenMP disabled)" OFF)

# Set build type based on whether USE_OPENMP is turned on
if(USE_OPENMP)
    set(CMAKE_BUILD_TYPE Release)  # Set to Release if using OpenMP
else()
    set(CMAKE_BUILD_TYPE Debug)    # Otherwise, set to Debug
endif()

if (USE_OPT_ARGS_ONLY)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Ofast -march=native")
endif()

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/..)
include_directories(${CMAKE_SOURCE_DIR})

# If USE_OPENMP is turned on, find OpenMP and set additional flags
if(USE_OPENMP)
    # Set OpenMP paths for macOS with Homebrew
    if(APPLE)
        set(OpenMP_C_FLAGS "-Xpreprocessor -fopenmp")
        set(OpenMP_C_LIB_NAMES "omp")
        set(OpenMP_omp_LIBRARY "/opt/homebrew/opt/libomp/lib/libomp.dylib")
        set(OpenMP_C_INCLUDE_DIRS "/opt/homebrew/opt/libomp/include")

        # Set OpenMP C++ flags and library names
        set(OpenMP_CXX_FLAGS "-Xpreprocessor -fopenmp")
        set(OpenMP_CXX_LIB_NAMES "omp")
        set(OpenMP_CXX_INCLUDE_DIRS "/opt/homebrew/opt/libomp/include")
    endif()

    find_package(OpenMP REQUIRED)  # Require OpenMP
    if(OpenMP_CXX_FOUND)
        add_definitions(-DUSE_OPENMP)
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Ofast -march=native")  # Add Ofast and march=native
    endif()
endif()

add_executable(run run.cpp)

# Link OpenMP if it was found and if USE_OPENMP is turned on
if(OpenMP_CXX_FOUND AND USE_OPENMP)
    target_link_libraries(run PUBLIC OpenMP::OpenMP_CXX)
endif()
