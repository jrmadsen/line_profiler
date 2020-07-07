cmake_minimum_required(VERSION 3.11 FATAL_ERROR)
project(line_profiler LANGUAGES C)

find_package(PythonInterp REQUIRED)

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake/Modules ${CMAKE_MODULE_PATH})

find_package(Cython ${TIMEMORY_FIND_REQUIREMENT})
if(NOT Cython_FOUND)
    return()
endif()

find_package(PythonExtensions REQUIRED)
find_package(PythonLibs REQUIRED)

add_subdirectory(line_profiler)

# install kernprof as the __main__
configure_file(${PROJECT_SOURCE_DIR}/kernprof.py
    ${CMAKE_BINARY_DIR}/${CMAKE_PROJECT_NAME}/line_profiler/__main__.py
    COPYONLY)
