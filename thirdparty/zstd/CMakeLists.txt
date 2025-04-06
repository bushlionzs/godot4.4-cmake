#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(ZSTD_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/zstd)

file(GLOB  ZSTD_MODULES_SOURCE 
    "${ZSTD_SOURCE_DIR}/*.c"
	"${ZSTD_SOURCE_DIR}/*.h"
	"${ZSTD_SOURCE_DIR}/common/*.c"
	"${ZSTD_SOURCE_DIR}/common/*.h"
	"${ZSTD_SOURCE_DIR}/compress/*.c"
	"${ZSTD_SOURCE_DIR}/compress/*.h"
	"${ZSTD_SOURCE_DIR}/decompress/*.c"
	"${ZSTD_SOURCE_DIR}/decompress/*.h"
	)

foreach(file ${ZSTD_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(zstd thirdparty STATIC ${ZSTD_MODULES_SOURCE})

target_include_directories(zstd PUBLIC 
    ${ZSTD_SOURCE_DIR}
	)
#target_link_libraries(etcpak zlib) 
