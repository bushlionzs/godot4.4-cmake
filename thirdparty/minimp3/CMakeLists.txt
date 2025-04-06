#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(MINIMP3_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/minimp3)

file(GLOB  MINIMP3_MODULES_SOURCE
    "${MINIMP3_SOURCE_DIR}/*.c"
	"${MINIMP3_SOURCE_DIR}/*.h"
	)

foreach(file ${MINIMP3_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(minimp3 thirdparty STATIC ${MINIMP3_MODULES_SOURCE})

target_include_directories(minimp3 PUBLIC 
    ${MINIMP3_SOURCE_DIR}
	)
#target_link_libraries(minizip zlib) 
