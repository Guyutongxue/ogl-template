cmake_minimum_required(VERSION 3.15)

project(OglTemplate)

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

add_subdirectory(src)