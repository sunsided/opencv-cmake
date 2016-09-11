project("OpenCV")

cmake_minimum_required(VERSION 2.8)

# conan.io package manager .. because awesome
if(EXISTS ${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
    include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
    conan_basic_setup()
endif()

# use a sane install directory
set(CMAKE_INSTALL_PREFIX "${CMAKE_CURRENT_LIST_DIR}/install" CACHE PATH "Default installation path" FORCE)

# set some default options
set(BUILD_opencv_legacy OFF CACHE BOOL "")
set(BUILD_opencv_videostab OFF CACHE BOOL "")
set(BUILD_opencv_world OFF CACHE BOOL "")
set(BUILD_opencv_superres OFF CACHE BOOL "")
set(BUILD_opencv_examples ON CACHE BOOL "")
set(BUILD_opencv_xfeatures2d ON CACHE BOOL "")

# Bugfixes
# Visual Studio 2013 doesn't support OpenMP 3.0+ nicht, which is required for unsigned types in for loops
set(BUILD_opencv_surface_matching OFF CACHE BOOL "")

set(BUILD_EXAMPLES ON CACHE BOOL "")
set(BUILD_OPENEXR OFF CACHE BOOL "")
set(BUILD_PERF_TESTS OFF CACHE BOOL "")
set(BUILD_TESTS OFF CACHE BOOL "")
set(BUILD_JASPER ON CACHE BOOL "JPEG2000")
set(WITH_OPENMP ON CACHE BOOL "")

# patch in the contrib module
set(OPENCV_EXTRA_MODULES_PATH "${CMAKE_CURRENT_LIST_DIR}/contrib/modules" CACHE STRING "Pfad zu den Contrib-Modulen")

# patch in the actual OpenCV sources
add_subdirectory(opencv)
