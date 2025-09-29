cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 1.5.1)
set(toolchainVersion 13_2_Rel1)
set(picotoolVersion 2.0.0)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# =====================================================


set(PICO_BOARD pico_w CACHE STRING "Board type")


include(pico_sdk_import.cmake)

project(monitoramento-temp-hum C CXX ASM)


pico_sdk_init()


add_executable(monitoramento-temp-hum
    monitoramento-temp-hum.c   
    aht10/aht10.c
    pico_ssd1306/ssd1306.c                  
)


pico_set_program_name(monitoramento-temp-hum "monitoramento-temp-hum")
pico_set_program_version(monitoramento-temp-hum "0.1")


pico_enable_stdio_uart(monitoramento-temp-hum 0)
pico_enable_stdio_usb(monitoramento-temp-hum 1)


target_include_directories(monitoramento-temp-hum PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}
    ${CMAKE_CURRENT_LIST_DIR}/aht10
    ${CMAKE_CURRENT_LIST_DIR}/pico_ssd1306
)


target_link_libraries(monitoramento-temp-hum
    pico_stdlib
    hardware_i2c 
)


pico_add_extra_outputs(monitoramento-temp-hum)
