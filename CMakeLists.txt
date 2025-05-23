﻿cmake_minimum_required(VERSION 3.8)
project("raymedia")

# This section configures MSVC (Visual Studio) to enable "Edit and Continue" debugging
if(POLICY CMP0141)
    cmake_policy(SET CMP0141 NEW)
    set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

# Determine the project's root directory and check if it's the main project
set(RMEDIA_ROOT_PATH ${CMAKE_CURRENT_SOURCE_DIR})

if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    set(RMEDIA_IS_MAIN ON)
else()
    set(RMEDIA_IS_MAIN OFF)
endif()

# Project configuration options
option(RMEDIA_BUILD_SHARED "Build raymedia as a shared library" OFF)
option(RMEDIA_BUILD_EXAMPLES "Build the examples" ${RMEDIA_IS_MAIN})

# Add the CMake module path for custom FindFFMPEG.cmake and FindRaylib.cmake
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/CMakeModules/")

# Find FFmpeg
find_package(FFMPEG REQUIRED)

# Find Raylib
find_package(RAYLIB REQUIRED)

if(NOT RAYLIB_FOUND)
    message(FATAL_ERROR "Raylib not found! Install it or provide custom paths using -DRAYLIB_INCLUDE_DIR and -DRAYLIB_LIBRARY_DIR")
else()
    message(STATUS "Raylib found and linked: ${RAYLIB_LIBRARY}")
endif()

# Set the library type
set(RMEDIA_LIB_TYPE STATIC)
if(RMEDIA_BUILD_SHARED)
    set(RMEDIA_LIB_TYPE SHARED)
    add_definitions(-DRMEDIA_BUILD_SHARED)
endif()

# Create the raymedia library
add_library(${PROJECT_NAME} ${RMEDIA_LIB_TYPE} "${RMEDIA_ROOT_PATH}/src/rmedia.c")

# Link project dependencies
if(FFMPEG_FOUND AND RAYLIB_FOUND)
    target_include_directories(${PROJECT_NAME} PUBLIC "${CMAKE_SOURCE_DIR}/src" ${FFMPEG_INCLUDE_DIRS} ${RAYLIB_INCLUDE_DIR})
    target_link_libraries(${PROJECT_NAME} PUBLIC 
        ${RAYLIB_LIBRARY} ${FFMPEG_LIBRARIES}
    )

    # Link Windows Multimedia API if on Windows
    if(WIN32)
        target_link_libraries(${PROJECT_NAME} PUBLIC winmm)
    endif()
else()
    message(FATAL_ERROR "FFmpeg or Raylib not found. Please install them or provide the correct paths.")
endif()

# Ensure the C standard is set for MSVC if using CMake version greater than 3.12
if(CMAKE_VERSION VERSION_GREATER 3.12)
    set_property(TARGET raymedia PROPERTY C_STANDARD 99)
endif()

# Example configuration
if(RMEDIA_BUILD_EXAMPLES)
    add_subdirectory("${RMEDIA_ROOT_PATH}/examples/media")
endif()