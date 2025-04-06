#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(LIBVORBIS_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/libvorbis)

file(GLOB  LIBVORBIS_MODULES_SOURCE 
    "${LIBVORBIS_SOURCE_DIR}/*.c"
	"${LIBVORBIS_SOURCE_DIR}/*.h"
	)


list(REMOVE_ITEM LIBVORBIS_MODULES_SOURCE "${LIBVORBIS_SOURCE_DIR}/analysis.c")
list(REMOVE_ITEM LIBVORBIS_MODULES_SOURCE "${LIBVORBIS_SOURCE_DIR}/barkmel.c")
list(REMOVE_ITEM LIBVORBIS_MODULES_SOURCE "${LIBVORBIS_SOURCE_DIR}/psytune.c")
list(REMOVE_ITEM LIBVORBIS_MODULES_SOURCE "${LIBVORBIS_SOURCE_DIR}/tone.c")
list(REMOVE_ITEM LIBVORBIS_MODULES_SOURCE "${LIBVORBIS_SOURCE_DIR}/vorbisenc.c")

foreach(file ${LIBVORBIS_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(libvorbis thirdparty STATIC ${LIBVORBIS_MODULES_SOURCE})

target_include_directories(libvorbis PUBLIC 
    ${LIBVORBIS_SOURCE_DIR}
	)
target_link_libraries(libvorbis libogg) 
