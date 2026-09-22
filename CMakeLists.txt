#
# @file CMakeLists.txt
#
# @brief Principal CMake configuration file
#
# @authors
# @author Guillaume Le Bars <guillaume.lebars@epfl.ch>
#
cmake_minimum_required (VERSION 3.15.0)

project(forsisl)

enable_language(Fortran)
enable_language(C)

# Search and load the FUTILS configuration file
find_package(sisl REQUIRED)

find_package(OpenMP REQUIRED)

# Compiler flags for debug/optimization
if(${CMAKE_Fortran_COMPILER_ID} MATCHES "Intel")
  set(CMAKE_AR ${XIAR})
  set(CMAKE_Fortran_FLAGS_DEBUG "${CMAKE_Fortran_FLAGS_DEBUG} -traceback -check bounds -warn unused -fpp")
  set(CMAKE_Fortran_FLAGS_RELEASE "${CMAKE_Fortran_FLAGS_RELEASE} -xHost -fpp")
elseif(${CMAKE_Fortran_COMPILER_ID} MATCHES "GNU")
  if(CMAKE_Fortran_COMPILER_VERSION VERSION_GREATER_EQUAL 10.0.0)
    set(CMAKE_Fortran_FLAGS_DEBUG "${CMAKE_Fortran_FLAGS_DEBUG} -fallow-argument-mismatch -cpp")
  endif()
  set(CMAKE_Fortran_FLAGS_DEBUG "${CMAKE_Fortran_FLAGS_DEBUG} -fbounds-check -fbacktrace -cpp")
endif()

# Add futils source and examples/tests directories
add_subdirectory(src)

include(GNUInstallDirs)
if(NOT CMAKE_RUNTIME_OUTPUT_DIRECTORY)
  set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_INSTALL_BINDIR})
endif()
if(NOT CMAKE_LIBRARY_OUTPUT_DIRECTORY)
  set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
endif()
if(NOT CMAKE_ARCHIVE_OUTPUT_DIRECTORY)
  set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
endif()

# Installation root directory
if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
 if($ENV{PREFIX})
    set(${CMAKE_INSTALL_PREFIX} $ENV{PREFIX})
  else()
    set(CMAKE_INSTALL_PREFIX ${CMAKE_CURRENT_SOURCE_DIR} CACHE PATH "..." FORCE)
  endif()
  message(STATUS "CMAKE_INSTALL_PREFIX is " ${CMAKE_INSTALL_PREFIX})
endif()

