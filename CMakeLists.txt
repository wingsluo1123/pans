cmake_minimum_required(VERSION 3.22.1) #指定最低的cmake版本

# 指定编译器
set(CMAKE_C_COMPILER "/usr/bin/gcc" CACHE FILEPATH "c compiler" FORCE)
set(CMAKE_CXX_COMPILER "/usr/bin/gcc" CACHE FILEPATH "c++ compiler" FORCE)

# 前两行设置使用C++20标准，并强制编译器支持，否则报错；第三行允许编译器扩展，可能降低可移植性，问题不大先开着
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)

# 定义项目
project(pans
VERSION 0.1.0
LANGUAGES CXX C)

#配套分发支持
include(GNUInstallDirs) # 避免硬编码
include(CMakePackageConfigHelpers) # 创建文件的工具集 比如这个pans项目作为一个三方库给别的项目使用，这个工具集可以正确的发现头文件和一些依赖

option(PANS_BUILD_EXAMPLES "Build examples" ON)
option(PANS_BUILD_TESTS    "Build tests"    ON)
option(PANS_BUILD_TOOLS    "Build tools"    ON)

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE debug CACHE STRING "Choose the type of build." FORCE)
endif()
set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS Debug Release RelWithDeInfo MinSizeRel)
message(STATUS "Build Type: ${CMAKE_BUILD_TYPE}")

cmake_host_system_information(RESULT CPU_NUMS QUERY NUMBER_OF_LOGICAL_CORES)
message(STATUS "CPU NUMS on current server is : ${CPU_NUMS}")
math(EXPR JOBS "${CPU_NUMS} + ${CPU_NUMS} / 2")
set(CMAKE_BUILD_PARALLEL_LEVEL ${JOBS} CACHE STRING "并行编译线程数")
message(STATUS "Compiler default work jobs: ${CMAKE_BUILD_PARALLEL_LEVEL}")


