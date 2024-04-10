cmake_minimum_required(VERSION 3.27)

project("test-space")
message(STATUS "Project name: ${CMAKE_PROJECT_NAME}")

set(CMAKE_CXX_STANDARD 17)

add_executable(${CMAKE_PROJECT_NAME} main.cpp)
