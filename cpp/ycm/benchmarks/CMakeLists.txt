# Copyright (C) 2017 ycmd contributors
#
# This file is part of ycmd.
#
# ycmd is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# ycmd is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with ycmd.  If not, see <http://www.gnu.org/licenses/>.

project( ycm_core_benchmarks )
cmake_minimum_required( VERSION 3.14 )

# We don't want to test the benchmark library.
set( BENCHMARK_ENABLE_TESTING
     OFF CACHE BOOL "Enable testing of the benchmark library" )
set( BUILD_SHARED_LIBS OFF )

add_subdirectory( benchmark )
set( BENCHMARK_INCLUDE_DIRS ${benchmark_SOURCE_DIR}/include )
set( BENCHMARK_LIBRARIES benchmark )

file( GLOB SOURCES *.h *.cpp )

add_executable( ${PROJECT_NAME}
                ${SOURCES} )

target_include_directories( ${PROJECT_NAME} PRIVATE ${BENCHMARK_INCLUDE_DIRS} )

if( MSVC )
  # Build benchmark and ycm_core_benchmarks targets in cmake ycm/benchmarks
  # folder.
  foreach( OUTPUTCONFIG ${CMAKE_CONFIGURATION_TYPES} )
    string( TOUPPER ${OUTPUTCONFIG} OUTPUTCONFIG )
    set_target_properties( ${BENCHMARK_LIBRARIES} PROPERTIES
      RUNTIME_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${PROJECT_BINARY_DIR} )
    set_target_properties( ${PROJECT_NAME} PROPERTIES
      RUNTIME_OUTPUT_DIRECTORY_${OUTPUTCONFIG} ${PROJECT_BINARY_DIR} )
  endforeach()
endif()

target_link_libraries( ${PROJECT_NAME}
                       PRIVATE ycm_core
                       PRIVATE ${BENCHMARK_LIBRARIES} )
