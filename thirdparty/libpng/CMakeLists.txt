#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(LIBPNG_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/libpng)

file(GLOB  LIBPNG_MODULES_SOURCE 
    "${LIBPNG_SOURCE_DIR}/*.c"
	"${LIBPNG_SOURCE_DIR}/*.h"
	"${LIBPNG_SOURCE_DIR}/intel/*.c"
	"${LIBPNG_SOURCE_DIR}/intel/*.h"
	)

foreach(file ${LIBPNG_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(libpng thirdparty STATIC ${LIBPNG_MODULES_SOURCE})

target_include_directories(libpng PUBLIC 
    ${LIBPNG_SOURCE_DIR}
	)
target_link_libraries(libpng zlib) 
