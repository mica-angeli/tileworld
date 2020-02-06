find_package(SDL REQUIRED)
include_directories(${SDL_INCLUDE_DIR} "${CMAKE_CURRENT_SOURCE_DIR}")

set(OSHW_SDL_SOURCES
    sdlerr.c sdlin.c sdloshw.c sdlout.c sdlsfx.c sdltext.c sdltile.c sdltimer.c
)

add_library(oshw-sdl STATIC ${OSHW_SDL_SOURCES})
target_link_libraries(oshw-sdl ${SDL_LIBRARY})
