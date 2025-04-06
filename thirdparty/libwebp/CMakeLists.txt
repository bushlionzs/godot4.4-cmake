#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(LIBWEBP_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/libwebp)

file(GLOB  LIBWEBP_MODULES_SOURCE
    "${LIBWEBP_SOURCE_DIR}/src/utils/*.c"
	"${LIBWEBP_SOURCE_DIR}/src/utils/*.h"
	"${LIBWEBP_SOURCE_DIR}/src/mux/*.c"
	"${LIBWEBP_SOURCE_DIR}/src/mux/*.h"
	"${LIBWEBP_SOURCE_DIR}/src/enc/*.c"
	"${LIBWEBP_SOURCE_DIR}/src/enc/*.h"
	"${LIBWEBP_SOURCE_DIR}/src/dsp/*.c"
	"${LIBWEBP_SOURCE_DIR}/src/dsp/*.h"
	"${LIBWEBP_SOURCE_DIR}/src/demux/*.c"
	"${LIBWEBP_SOURCE_DIR}/src/demux/*.h"
	"${LIBWEBP_SOURCE_DIR}/src/dec/*.c"
	"${LIBWEBP_SOURCE_DIR}/src/dec/*.h"
	"${LIBWEBP_SOURCE_DIR}/sharpyuv/*.c"
	"${LIBWEBP_SOURCE_DIR}/sharpyuv/*.h"
	)

foreach(file ${LIBWEBP_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(libwebp thirdparty STATIC ${LIBWEBP_MODULES_SOURCE})
target_include_directories(libwebp PRIVATE ${LIBWEBP_SOURCE_DIR})
target_include_directories(libwebp PUBLIC ${LIBWEBP_SOURCE_DIR}/src)

