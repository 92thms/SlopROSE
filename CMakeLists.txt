cmake_minimum_required(VERSION 3.20)
project(SlopROSEServers CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug)
endif()

add_compile_options(-Wall -Wextra)

# M1 scaffolding: this is expected to fail to compile until the M2
# portability layer replaces the Windows API surface (<windows.h>,
# IOCP, ODBC, WinINet, ...). The goal here is a correct build graph,
# not a clean build.
add_subdirectory(Sources/LIB_Util)
add_subdirectory(Sources/LIB_Server)
add_subdirectory(Sources/Server/SHO_GS)
add_subdirectory(Sources/Server/SHO_LS)
add_subdirectory(Sources/Server/SHO_WS)
