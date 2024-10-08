cmake_minimum_required(VERSION 3.22)

#
# This file is generated only once,
# and is not re-generated if converter is called multiple times.
#
# User is free to modify the file as much as necessary
#

set(APP_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../app")
set(DRIVERS_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../drivers")
set(LIBS_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../libs")

# Setup compiler settings
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)


# Define the build type
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

# Set the project name
set(CMAKE_PROJECT_NAME PortableSigGen_g474re_rtos)

# Include toolchain file
include("cmake/gcc-arm-none-eabi.cmake")

# Enable compile command to ease indexing with e.g. clangd
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

# Enable CMake support for ASM and C languages
enable_language(C ASM)

# Core project settings
project(${CMAKE_PROJECT_NAME})
message("Build type: " ${CMAKE_BUILD_TYPE})

# Create an executable object type
add_executable(${CMAKE_PROJECT_NAME})

# Add STM32CubeMX generated sources
add_subdirectory(cmake/stm32hal)
add_subdirectory(cmake/freertos)
add_subdirectory(cmake/drivers)
add_subdirectory(cmake/systemview)
add_subdirectory(cmake/printf)
add_subdirectory(cmake/ulog)
# add_subdirectory(cmake/app)

# Link directories setup
target_link_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined library search paths
)

# Add sources to executable
target_sources(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user sources here
)

# Add include paths
target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined include paths
)

# Add project symbols (macros)
target_compile_definitions(${CMAKE_PROJECT_NAME} PRIVATE
ULOG_ENABLED
    # Add user defined symbols
)

target_link_libraries(systemview PRIVATE freertos)
target_link_libraries(freertos PRIVATE systemview)

# Add linked libraries
target_link_libraries(${CMAKE_PROJECT_NAME}
    # stm32cubemx
    stm32hal
    systemview
    freertos
    drivers
    printf
    ulog
)





