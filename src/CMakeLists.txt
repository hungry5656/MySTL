cmake_minimum_required(VERSION 3.13)
project(STLSrc)
set(CMAKE_CXX_STANDARD 14)

set(SRC_LIB_NAME STLSrc)
set(EXE_NAME ManualTesting)


add_library(${SRC_LIB_NAME} STATIC type_traits.h)
target_include_directories(${SRC_LIB_NAME}  PUBLIC .)
target_compile_options(${SRC_LIB_NAME}  PRIVATE -Wall -Wextra -Werror)

add_executable(${EXE_NAME} ManualTesting.cpp)
target_link_libraries(${EXE_NAME} PRIVATE ${SRC_LIB_NAME})
target_compile_options(${EXE_NAME} PRIVATE -Wall -Wextra -Werror)