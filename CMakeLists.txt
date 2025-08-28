#
# This is a CMakeLists.txt file to compile a program against mathplot
#
cmake_minimum_required(VERSION 3.10)

# Give your project a name
project(mnist)

# mathplot uses c++-20 language features
set(CMAKE_CXX_STANDARD 20)

# Set up CMAKE_CXX_FLAGS
if (APPLE)
  set(CMAKE_CXX_FLAGS "-g -O3 -Wall -Wfatal-errors -DGL_SILENCE_DEPRECATION")
elseif (WIN32)
  set(CMAKE_CXX_FLAGS "-DNOMINMAX /EHsc /Zc:__cplusplus")
else()
  # This assumes a gcc compiler (or a gcc mimic like Clang)
  set(CMAKE_CXX_FLAGS "-g -O3 -Wall -Wextra -Wpedantic -pedantic-errors -Werror -Wfatal-errors -Wno-psabi -Wno-unknown-pragmas")
endif()

# Tell the program where the mathplot fonts are, to compile them into the binary
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DMPLOT_FONTS_DIR=\"\\\"${PROJECT_SOURCE_DIR}/mathplot/fonts\\\"\"")

# Find the libraries which will be needed by mathplot
set(OpenGL_GL_PREFERENCE "GLVND") # Following `cmake --help-policy CMP0072`
find_package(OpenGL REQUIRED)
find_package(glfw3 3.3 REQUIRED)
find_package(Freetype REQUIRED)
find_package(nlohmann_json REQUIRED)

# include files for graphics
include_directories(${OPENGL_INCLUDE_DIR} ${GLFW3_INCLUDE_DIR} ${FREETYPE_INCLUDE_DIRS})
# Assuming that you installed mathplot in-tree (i.e. 'next to' yourprog.cpp).
set(MPLOT_INCLUDE_PATH "${PROJECT_SOURCE_DIR}/mathplot" CACHE PATH "The path to mathplot")
include_directories(BEFORE ${MPLOT_INCLUDE_PATH})         # Allows mplot/Header.h to be found
include_directories(BEFORE ${MPLOT_INCLUDE_PATH}/maths)   # Allows sm/Header.h to be found

#
# All the library finding is now done. It's time to compile the program.
#

# The example is compiled from a single c++ file
add_executable(grid_mnist grid_mnist.cpp)
# Mathplot code requires a number of libraries, these are the ones required for graphics
target_link_libraries(grid_mnist OpenGL::GL Freetype::Freetype glfw)

#
# All done!
#

# This is just a useful stanza for debugging of cmake variables:
option(DEBUG_VARIABLES OFF)
if(DEBUG_VARIABLES)
  get_cmake_property(_variableNames VARIABLES)
  foreach (_variableName ${_variableNames})
    message(STATUS "${_variableName}=${${_variableName}}")
  endforeach()
endif(DEBUG_VARIABLES)
