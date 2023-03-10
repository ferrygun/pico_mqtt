cmake_minimum_required(VERSION 3.13)
 
set(PICO_BOARD pico_w)
 
include(pico_sdk_import.cmake)
 
project(pico_motion_detection_project C CXX ASM) 
set(CMAKE_C_STANDARD 11) 
set(CMAKE_CXX_STANDARD 17) 
if (PICO_SDK_VERSION_STRING VERSION_LESS "1.3.0")
    message(FATAL_ERROR "Raspberry Pi Pico SDK version 1.3.0 (or later) required. Your version is ${PICO_SDK_VERSION_STRING}")
endif()
pico_sdk_init()

add_compile_options(-Wall
        -Wno-format          # int != int32_t as far as the compiler is concerned because gcc has int32_t as long int
        -Wno-unused-function # we have some for the docs that aren't called
        -Wno-maybe-uninitialized
        )
 
add_executable(app 
    source/main.cpp
)
 
 
pico_enable_stdio_usb(app 1)

target_link_libraries(app
                pico_stdlib
                hardware_adc
                pico_cyw43_arch_lwip_threadsafe_background
                pico_stdlib
                pico_lwip_mbedtls
                pico_mbedtls
                pico_lwip_mqtt
            )




pico_add_extra_outputs(app)
