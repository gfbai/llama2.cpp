cmake_minimum_required(VERSION 3.16)
project(llama2)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/..)
include_directories(${CMAKE_SOURCE_DIR})
add_executable(run run.cpp)


