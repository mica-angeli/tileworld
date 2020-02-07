cmake_minimum_required(VERSION 2.6)
project(tworld)

if(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX)
    set(CMAKE_C_FLAGS "-Wall -Wextra ${CMAKE_C_FLAGS}")
    set(CMAKE_CXX_FLAGS "-Wall -Wextra ${CMAKE_CXX_FLAGS}")
endif()

include_directories("${CMAKE_CURRENT_SOURCE_DIR}"
                    "${CMAKE_CURRENT_BINARY_DIR}")

if(NOT WIN32 AND NOT APPLE)
    # Standard location for *nix systems
    add_definitions("-DROOTDIR=\"${CMAKE_INSTALL_PREFIX}/share/tworld\"")
endif()

# Add the OS/HW abstraction
if(OSHW_USE_SDL2)
    add_subdirectory(oshw-sdl2)
else()
    add_subdirectory(oshw-sdl)
endif()

# Core TWorld sources
set(TWorld_SOURCES
    tworld.c series.c play.c encoding.c solution.c res.c lxlogic.c mslogic.c
    unslist.c help.c score.c random.c cmdline.c fileio.c err.c comptime.h
    messages.c
)

# Auto-generate comptime.h
if(WIN32)
    add_custom_command(OUTPUT "${CMAKE_CURRENT_BINARY_DIR}/comptime.h"
                       COMMAND echo \#define COMPILE_TIME \"%DATE% %TIME%\"
                               > "${CMAKE_CURRENT_BINARY_DIR}/comptime.h")
else()
    add_custom_command(OUTPUT "${CMAKE_CURRENT_BINARY_DIR}/comptime.h"
                       COMMAND echo \\\#define COMPILE_TIME \\\"`date '+%Y %b %e %T %Z'`\\\"
                               > "${CMAKE_CURRENT_BINARY_DIR}/comptime.h")
endif()

# Tile world executable
set(TW_NAME tworld)

add_executable("${TW_NAME}" WIN32 ${TWorld_SOURCES})
if(OSHW_USE_SDL2)
    target_link_libraries("${TW_NAME}" oshw-sdl2)
else()
    target_link_libraries("${TW_NAME}" oshw-sdl)
endif()

# Installation parameters
set(TWORLD_DATA
    "${CMAKE_CURRENT_SOURCE_DIR}/data/intro.dat"
)

set(TWORLD_SETS
    "${CMAKE_CURRENT_SOURCE_DIR}/sets/intro-lynx.dac"
    "${CMAKE_CURRENT_SOURCE_DIR}/sets/intro-ms.dac"
    "${CMAKE_CURRENT_SOURCE_DIR}/sets/cc-lynx.dac"
    "${CMAKE_CURRENT_SOURCE_DIR}/sets/cc-ms.dac"
    "${CMAKE_CURRENT_SOURCE_DIR}/sets/CCLP2.dac"
)

set(TWORLD_RES
    "${CMAKE_CURRENT_SOURCE_DIR}/res/rc"

    "${CMAKE_CURRENT_SOURCE_DIR}/res/atiles.bmp"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/tiles.bmp"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/font.bmp"

    "${CMAKE_CURRENT_SOURCE_DIR}/res/block.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/bomb.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/bump.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/chack.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/click.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/crackle.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/death.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/derezz.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/ding.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/door.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/force.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/oof.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/plip.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/popup.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/skate.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/skaturn.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/slurp.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/snick.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/socket.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/splash.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/tada.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/teleport.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/thief.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/tick.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/ting.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/traphit.wav"
    "${CMAKE_CURRENT_SOURCE_DIR}/res/whisk.wav"

    "${CMAKE_CURRENT_SOURCE_DIR}/res/unslist.txt"
)

install(TARGETS "${TW_NAME}"
        RUNTIME DESTINATION bin
)
install(FILES "${CMAKE_CURRENT_SOURCE_DIR}/docs/tworld.6"
        DESTINATION share/man
)
install(FILES ${TWORLD_DATA}
        DESTINATION share/tworld/data
)
install(FILES ${TWORLD_SETS}
        DESTINATION share/tworld/sets
)
install(FILES ${TWORLD_RES}
        DESTINATION share/tworld/res
)
