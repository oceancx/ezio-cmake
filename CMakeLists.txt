cmake_minimum_required(VERSION 3.5)

project(ezio CXX)

#add_subdirectory(kbase)


set(KBase_DIR ${CMAKE_SOURCE_DIR}/cmake/lib/cmake/KBase)
mark_as_advanced(KBase_DIR)
find_package(KBase 1.0.0 REQUIRED)

add_subdirectory(src)
