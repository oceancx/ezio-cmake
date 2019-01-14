cmake_minimum_required(VERSION 3.5)
project(Test CXX)


list(INSERT CMAKE_PREFIX_PATH 0  ../install)
find_package(KBase 1.0.0 REQUIRED)
find_package(EZIO 1.0.0 REQUIRED)


add_executable(Test main.cpp)
target_link_libraries(Test PUBLIC EZIO::EZIO)
