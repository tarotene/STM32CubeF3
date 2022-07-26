# ------------------------------------------------------------------------------
# Directories
# ------------------------------------------------------------------------------
set(HAL_Driver ${CMAKE_CURRENT_SOURCE_DIR}/Drivers/STM32F3xx_HAL_Driver)
set(CMSIS_Device ${CMAKE_CURRENT_SOURCE_DIR}/Drivers/CMSIS/Device)
set(CMSIS_Core ${CMAKE_CURRENT_SOURCE_DIR}/Drivers/CMSIS/Core)

# ------------------------------------------------------------------------------
# Version
# ------------------------------------------------------------------------------
cmake_minimum_required(VERSION 3.23)

# ------------------------------------------------------------------------------
# Toolchain
# ------------------------------------------------------------------------------
set(TOOLCHAIN_PREFIX "/usr/local")
set(CMAKE_TOOLCHAIN_FILE
    ${CMAKE_CURRENT_SOURCE_DIR}/external/cmake-arm-embedded/toolchain-arm-none-eabi.cmake
)

# ------------------------------------------------------------------------------
# Project name (stored in variable `PROJECT_NAME`)
#
# see: https://cmake.org/cmake/help/latest/command/project.html
# ------------------------------------------------------------------------------
project(stm32f3xx_hal)

# ------------------------------------------------------------------------------
# Object definition
#
# see: https://cmake.org/cmake/help/latest/command/add_library.html
# ------------------------------------------------------------------------------
add_library(${PROJECT_NAME} OBJECT ${HAL_Driver}/Src/stm32f3xx_hal.c)

# ------------------------------------------------------------------------------
# Include directories
#
# see:
# https://cmake.org/cmake/help/latest/command/target_include_directories.html
# ------------------------------------------------------------------------------
target_include_directories(${PROJECT_NAME} PRIVATE ${HAL_Driver}/Inc)
target_include_directories(${PROJECT_NAME} PRIVATE ${HAL_Driver}/Inc/Legacy)
target_include_directories(${PROJECT_NAME}
                           PRIVATE ${CMSIS_Device}/ST/STM32F3xx/Include)
target_include_directories(${PROJECT_NAME} PRIVATE ${CMSIS_Core}/Include)
target_include_directories(${PROJECT_NAME}
                           PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/conf)

# ------------------------------------------------------------------------------
# Compile definitions
#
# see:
# https://cmake.org/cmake/help/latest/command/target_compile_definitions.html
# ------------------------------------------------------------------------------
target_compile_definitions(${PROJECT_NAME} PRIVATE USE_HAL_DRIVER)
target_compile_definitions(${PROJECT_NAME} PRIVATE STM32F303x8)

# ------------------------------------------------------------------------------
# Delegetion to cmake-arm-embedded (variable LINKER_SCRIPT must be set)
# ------------------------------------------------------------------------------
set(LINKER_SCRIPT
    "${CMAKE_CURRENT_SOURCE_DIR}/Projects/STM32F303K8-Nucleo/Templates/SW4STM32/STM32F303K8_NUCLEO/STM32F303K8_NUCLEOTx_FLASH.ld"
)

include(external/cmake-arm-embedded/stm32f303k8.cmake)
