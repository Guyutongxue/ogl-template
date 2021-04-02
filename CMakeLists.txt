cmake_minimum_required(VERSION 3.15)

project(OglTemplate)

# I'm using GCC 11 now, while conan doesn't support yet
set(CONAN_DISABLE_CHECK_COMPILER 1)
include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

add_subdirectory(src)