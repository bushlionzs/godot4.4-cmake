#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(ASTCENC_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/astcenc)

file(GLOB  ASTCENC_MODULES_SOURCE 
    "${ASTCENC_SOURCE_DIR}/*.cpp"
	"${ASTCENC_SOURCE_DIR}/*.h"
	)

foreach(file ${ASTCENC_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(astcenc thirdparty STATIC ${ASTCENC_MODULES_SOURCE})

target_include_directories(astcenc PUBLIC 
    ${ASTCENC_SOURCE_DIR}
	)
#target_link_libraries(etcpak zlib) 
