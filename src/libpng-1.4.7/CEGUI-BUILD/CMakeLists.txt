set(DEPNAME libpng)

file(GLOB SOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} "../png*.c")
list(REMOVE_ITEM SOURCES ../pnggccrd.c ../pngvcrd.c ../pngtest.c)

set(HEADERS png.h pngconf.h pnglibconf.h)

include_directories(../ ${CMAKE_BINARY_DIR}/include)

if (NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/../pnglibconf.h")
    execute_process(COMMAND ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_SOURCE_DIR}/../scripts/pnglibconf.h.prebuilt"
                                                     "${CMAKE_CURRENT_SOURCE_DIR}/../pnglibconf.h")
endif()

cegui_add_library(${DEPNAME} SOURCES NONE)

set_target_properties(${DEPNAME} PROPERTIES
    COMPILE_DEFINITIONS "PNG_DLL;PNG_NO_MMX_CODE")

target_link_libraries(${DEPNAME} zlib)


set_target_properties(${DEPNAME}_static PROPERTIES
    COMPILE_DEFINITIONS PNG_NO_MMX_CODE)

foreach(_H_FILE ${HEADERS})
    add_custom_command(TARGET ${DEPNAME} POST_BUILD 
                       COMMAND ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_SOURCE_DIR}/../${_H_FILE}"
                                                        "${CMAKE_BINARY_DIR}/include/${_H_FILE}")
endforeach()

