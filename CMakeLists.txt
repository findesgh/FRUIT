cmake_minimum_required(VERSION 3.11.1)
project(FRUIT VERSION 3.4.3)
enable_language(Fortran)

# build Release by default
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Release CACHE INTERNAL
        "Set the default CMake build type to Release" FORCE)
endif(NOT CMAKE_BUILD_TYPE)

macro(add_Fortran_compiler_flags FLAGS)
  set(CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} ${FLAGS}")
endmacro(add_Fortran_compiler_flags)

# standard flags
add_Fortran_compiler_flags("-Wall -cpp")

if(${CMAKE_BUILD_TYPE} STREQUAL "Release")
  add_Fortran_compiler_flags("-O3")
elseif(${CMAKE_BUILD_TYPE} STREQUAL "Debug")
  add_Fortran_compiler_flags("-O0 -g")
endif(${CMAKE_BUILD_TYPE} STREQUAL "Release")

# put Fortran mod files into include
set(CMAKE_Fortran_MODULE_DIRECTORY ${CMAKE_BINARY_DIR}/include)
# put generated library files into lib
set(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)

find_package(MPI)
if(MPI_Fortran_FOUND)
  message(STATUS "MPI found, will also build with MPI support.")
  include_directories(${MPI_Fortran_INCLUDE_PATH})
else(MPI_Fortran_FOUND)
  message(WARNING "MPI not found.")
endif(MPI_Fortran_FOUND)

add_subdirectory(src)
