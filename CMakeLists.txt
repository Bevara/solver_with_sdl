set (THIRD_PARTIES ${CMAKE_SOURCE_DIR}/filters/third_parties)
set (THIRD_PARTIES_BINARIES ${CMAKE_BINARY_DIR}/filters/third_parties)
set (GPAC_LOCATION ${THIRD_PARTIES}/gpac)
set (GPAC_BINARIES ${THIRD_PARTIES_BINARIES}/gpac_minimal)

set(VERSION "1")

set(CMAKE_BUILD_TYPE Release)
set(CMAKE_CXX_FLAGS "-O3 -Wall -Wextra")
set(CMAKE_C_FLAGS "-O3 -Wall -Wextra")

add_compile_definitions(GPAC_HAVE_CONFIG_H GPAC_DISABLE_DOC)


include(exports.cmake)

SET(solver_with_sdl_SRC
  event.c
  filter_register.c
  properties.cpp
  stats.c
  gpac.c
  gpac_help.c
  compositor_tools.c
)

SET(EXTERNAL_RUNTIME
  "['stringToUTF8','UTF8ToString','stackAlloc','addFunction','allocateUTF8OnStack','stackSave','getTempRet0','setTempRet0','FS','cwrap','ccall']"
)

add_executable(solver_with_sdl_${VERSION} ${solver_with_sdl_SRC})

SET(JS_RUNTIME
  ${CMAKE_CURRENT_SOURCE_DIR}/loader.js
)

set_target_properties( solver_with_sdl_${VERSION}
                            PROPERTIES
                                LINK_FLAGS "${USE_FLAGS} -s USE_SDL=2 -s USE_FREETYPE=1 -sALLOW_MEMORY_GROWTH -sWASM_BIGINT -s MAIN_MODULE=2 -s EXPORT_NAME=libgpac --no-entry -s MODULARIZE=1 -s EXPORTED_FUNCTIONS=[${EXPORTED_FUNCTIONS}] -s --extern-post-js=${JS_RUNTIME} EXPORTED_RUNTIME_METHODS=${EXTERNAL_RUNTIME} -s ERROR_ON_UNDEFINED_SYMBOLS=0"
                                )



target_link_libraries(solver_with_sdl_${VERSION}
  ${CMAKE_CURRENT_SOURCE_DIR}/lib/libgpac_static.a
)

target_include_directories(solver_with_sdl_${VERSION} PRIVATE
  ${CMAKE_CURRENT_SOURCE_DIR}/include
)
