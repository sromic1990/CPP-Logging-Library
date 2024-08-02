cmake_minimum_required(VERSION 3.10)

project(CPPLoggingLibrary)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED True)

# Define source directories
set(SOURCE_DIR "${CMAKE_SOURCE_DIR}/src")
set(INCLUDE_DIR "${CMAKE_SOURCE_DIR}/include")
set(TEST_SOURCE_DIR "${CMAKE_SOURCE_DIR}/tests")
set(EXTERNAL_DIR "${CMAKE_SOURCE_DIR}/external/CPP-Test-Library")

# Include directories
include_directories(
    "${INCLUDE_DIR}"
    "${EXTERNAL_DIR}/include"
)

# Collect all source files
file(GLOB SOURCES
    "${SOURCE_DIR}/*.cpp"
    "${TEST_SOURCE_DIR}/*.cpp"
)

# Add executable combining all source files
add_executable(Output ${SOURCES})

# Link the Output executable with the CPP-Test-Library
target_link_libraries(Output "${EXTERNAL_DIR}/lib/libCPPTestLibrary.a")

# Set platform-specific output directories
if (WIN32)
    message("Windows")
    if (CMAKE_GENERATOR STREQUAL "MinGW Makefiles")
        set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/bin)
        set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/lib)
        set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/lib)
    else()
        message("Windows MSVC")
        set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/windows/bin)
        set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/windows/lib)
        set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/windows/lib)
    endif()
elseif (APPLE)
    message("Mac OS")
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/mac/bin)
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/mac/lib)
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/mac/lib)
else()
    message("Linux/Other")
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/bin)
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/lib)
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/build/lib)
endif()
