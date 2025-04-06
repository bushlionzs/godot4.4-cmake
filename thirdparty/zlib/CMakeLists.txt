#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(ZLIB_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/zlib)

file(GLOB  ZLIB_MODULES_SOURCE 
    "${ZLIB_SOURCE_DIR}/*.c"
	"${ZLIB_SOURCE_DIR}/*.h"
	)

foreach(file ${ZLIB_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(zlib thirdparty STATIC ${ZLIB_MODULES_SOURCE})

target_include_directories(zlib PUBLIC 
    ${ZLIB_SOURCE_DIR}
	)
#target_link_libraries(etcpak zlib) 
