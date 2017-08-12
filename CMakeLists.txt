project("OpenCV")

cmake_minimum_required(VERSION 2.8)

# conan.io package manager .. because awesome
if(EXISTS ${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
    include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
    conan_basic_setup()
endif()

# disable some things I'll never use anyways ...
set(BUILD_EXAMPLES ON CACHE BOOL "")
set(BUILD_PERF_TESTS OFF CACHE BOOL "")
set(BUILD_TESTS OFF CACHE BOOL "")
set(BUILD_DOCS OFF CACHE BOOL "")

# enable some things I always set
set(WITH_OPENMP ON CACHE BOOL "")
set(WITH_TBB ON CACHE BOOL "")
set(WITH_MKL ON CACHE BOOL "")
set(MKL_WITH_TBB ON CACHE BOOL "")
set(MKL_WITH_OPENMP ON CACHE BOOL "")

# patch in the contrib module
set(OPENCV_EXTRA_MODULES_PATH "${CMAKE_CURRENT_LIST_DIR}/contrib/modules" CACHE STRING "Path to the contrib modules")

# patch in the actual OpenCV sources
add_subdirectory(opencv)
