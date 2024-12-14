file(GLOB SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.c)

add_library(core SHARED
    ${SOURCES}
)

find_program(PYTHON_INTERPRETER python)

if(NOT PYTHON_INTERPRETER)
    message(FATAL_ERROR "Python interpreter not found! Make sure Python is installed and in PATH.")
endif()

set(INPUT ${CMAKE_SOURCE_DIR}/core/package)
set(OUTPUT ${CMAKE_BINARY_DIR}/core/core.zip)
set(SCRIPT ${CMAKE_SOURCE_DIR}/dev/build_tools/zip_archive.py)

file(GLOB_RECURSE PACKAGE_FILES ${INPUT}/*)

add_custom_command(
    OUTPUT ${OUTPUT}
    COMMAND ${PYTHON_INTERPRETER} ${SCRIPT} ${INPUT} ${OUTPUT}
    DEPENDS ${PACKAGE_FILES}
    COMMENT "Archiving dart core package."
)

add_custom_target(create_core_package ALL DEPENDS ${OUTPUT})

# Installation
install(TARGETS core DESTINATION lib)
install(FILES ${OUTPUT} DESTINATION lib)
