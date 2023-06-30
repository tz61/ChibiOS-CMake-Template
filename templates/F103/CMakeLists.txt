cmake_minimum_required(VERSION 3.10)



include(config/toolchain.cmake)


set(CMAKE_CXX_STANDARD 11)

project(ChibiOS_F103 ASM C CXX)


include(config/compile_options.cmake)
set(CMAKE_VERBOSE_MAKEFILE OFF)

include(ChibiOS.cmake)



include_directories(.)
add_executable(ChibiOS_F103
        main.cpp
        usbcfg.c
        board.c
        ${CHIBIOS_SRC}
        )
get_directory_property(TARGET_LIST BUILDSYSTEM_TARGETS)
foreach (target ${TARGET_LIST})
    if (NOT ${target} STREQUAL "ChibiOS" AND NOT ${target} STREQUAL "CMSIS_DSP")  # OBJECT library doesn't have PRE_BUILD or POST_BUILD
        # Add ChibiOS files and common sources

        # Echo target dev board before link

        # Run size utility after link to show the size of elf file
        add_custom_command(TARGET ${target} POST_BUILD
                COMMAND ${CMAKE_SIZE_UTILITY} ${target}.elf
                )  # execute size utility to show size of executable file

        # Set unique output file
        set_target_properties(
                ${target} PROPERTIES
                OUTPUT_NAME ${target}.elf
        )  #
    endif ()
endforeach (target)