cmake_minimum_required(VERSION 3.12)

project(stm32f4-cmsis)

string(TOLOWER ${STM32_TARGET_DEVICE} STM32_TARGET_DEVICE_LOWER)

set(header_files
    ${CMAKE_CURRENT_SOURCE_DIR}/include/cmsis_armcc.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/cmsis_armclang.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/cmsis_compiler.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/cmsis_gcc.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/cmsis_iccarm.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/cmsis_version.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/core_cm4.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/stm32f4xx.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/${STM32_TARGET_DEVICE_LOWER}.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/system_stm32f4xx.h
    ${CMAKE_CURRENT_SOURCE_DIR}/include/tz_context.h)

if(NOT STM32_TARGET_DEVICE)
    message(FATAL_ERROR "Please select the target device using the 'STM32_TARGET_DEVICE' variable!")
endif()

if(NOT STM32_HSE_VALUE)
    message(WARNING "No HSE frequency specified, using 25 MHz!")
    set(STM32_HSE_VALUE 25000000)
endif()

add_library(stm32f4-cmsis STATIC src/system_stm32f4xx.c)
target_sources(stm32f4-cmsis PUBLIC ${header_files})
target_compile_definitions(stm32f4-cmsis PUBLIC ${STM32_TARGET_DEVICE} HSE_VALUE=${STM32_HSE_VALUE})
target_include_directories(stm32f4-cmsis PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
