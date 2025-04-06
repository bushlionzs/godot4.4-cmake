#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(LIBTHEORA_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/libtheora)

set(LIBTHEORA_MODULES_SOURCE 
    "${LIBTHEORA_SOURCE_DIR}/bitpack.c"
	"${LIBTHEORA_SOURCE_DIR}/decinfo.c"
	"${LIBTHEORA_SOURCE_DIR}/decode.c"
	"${LIBTHEORA_SOURCE_DIR}/dequant.c"
	"${LIBTHEORA_SOURCE_DIR}/fragment.c"
	"${LIBTHEORA_SOURCE_DIR}/huffdec.c"
	"${LIBTHEORA_SOURCE_DIR}/idct.c"
	"${LIBTHEORA_SOURCE_DIR}/info.c"
	"${LIBTHEORA_SOURCE_DIR}/internal.c"
	"${LIBTHEORA_SOURCE_DIR}/quant.c"
	"${LIBTHEORA_SOURCE_DIR}/state.c"
	)

foreach(file ${LIBTHEORA_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(libtheora thirdparty STATIC ${LIBTHEORA_MODULES_SOURCE})

target_include_directories(libtheora PUBLIC 
    ${LIBTHEORA_SOURCE_DIR}
	)
target_link_libraries(libtheora libogg) 
