cmake_minimum_required(VERSION 3.5)
project(line-descriptor)

find_package(OpenCV 4 QUIET)
if(NOT OpenCV_FOUND)
	find_package(OpenCV 3 QUIET)
endif()
if(NOT OpenCV_FOUND)
	find_package(OpenCV 2.4.3 QUIET)
endif()
if(NOT OpenCV_FOUND)
	message(FATAL_ERROR "OpenCV >= 2.4.3 not found.")
endif()

if(COMMAND cmake_policy)
  	cmake_policy(SET CMP0003 NEW)	
endif(COMMAND cmake_policy)
SET(BUILD_SHARED_LIBS ON)
SET(CMAKE_MODULE_PATH $ENV{CMAKE_MODULE_PATH})
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x -O3 -mtune=native -march=native")

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/build)
set(LIBRARY_OUTPUT_PATH    ${PROJECT_SOURCE_DIR}/lib)

include_directories(include ${OpenCV_INCLUDE_DIRS})
list(APPEND LINK_LIBS ${OpenCV_LIBS} )
file(GLOB_RECURSE all_include_files RELATIVE "${CMAKE_SOURCE_DIR}" *.h *.hpp)

link_directories(${CMAKE_SOURCE_DIR}/src/)
file(GLOB_RECURSE all_source_files RELATIVE "${CMAKE_SOURCE_DIR}src/" *.cpp )

add_custom_target( linedesc_includes DEPENDS ${all_include_files} SOURCES ${all_source_files} )

add_library( linedesc ${all_source_files} )
target_link_libraries( linedesc ${LINK_LIBS} )

