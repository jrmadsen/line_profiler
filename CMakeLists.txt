cmake_minimum_required(VERSION 3.11 FATAL_ERROR)
project(line_profiler LANGUAGES C)

###
# Private helper function to execute `python -c "<cmd>"`
#
# Runs a python command and populates an outvar with the result of stdout.
# Be careful of indentation if `cmd` is multiline.
#
function(pycmd outvar cmd)
  execute_process(
    COMMAND "${PYTHON_EXECUTABLE}" -c "${cmd}"
    RESULT_VARIABLE _exitcode
    OUTPUT_VARIABLE _output)
  if(NOT ${_exitcode} EQUAL 0)
    message(ERROR "Failed when running python code: \"\"\"${cmd}\"\"\"")
    message(FATAL_ERROR "Python command failed with error code: ${_exitcode}")
  endif()
  # Remove supurflous newlines (artifacts of print)
  string(STRIP "${_output}" _output)
  set(${outvar} "${_output}" PARENT_SCOPE)
endfunction()


find_package(PythonInterp REQUIRED)

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake/Modules ${CMAKE_MODULE_PATH})

find_package(Cython REQUIRED)
find_package(PythonExtensions REQUIRED)
find_package(PythonLibs REQUIRED)

add_subdirectory(line_profiler)

# install kernprof as the __main__
configure_file(${PROJECT_SOURCE_DIR}/kernprof.py
    ${CMAKE_BINARY_DIR}/${CMAKE_PROJECT_NAME}/line_profiler/__main__.py
    COPYONLY)
