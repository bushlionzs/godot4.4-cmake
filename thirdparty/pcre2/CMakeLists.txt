#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
add_definitions(-DPCRE2_STATIC)
add_definitions(-DPCRE2_CODE_UNIT_WIDTH=32)
add_definitions(-DHAVE_CONFIG_H)
add_definitions(-DSUPPORT_UNICODE)
add_definitions(-DSUPPORT_JIT)
set(PCRE2_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/pcre2)

file(GLOB  PCRE2_MODULES_SOURCE 
    "${PCRE2_SOURCE_DIR}/src/*.c"
	"${PCRE2_SOURCE_DIR}/src/*.h"
	#"${PCRE2_SOURCE_DIR}/src/sljit/*.c"
	#"${PCRE2_SOURCE_DIR}/src/sljit/*.h"
	#"${PCRE2_SOURCE_DIR}/src/sljit/allocator_src/*.c"
	#"${PCRE2_SOURCE_DIR}/src/sljit/allocator_src/*.h"
	)

list(REMOVE_ITEM PCRE2_MODULES_SOURCE "${PCRE2_SOURCE_DIR}/src/pcre2_jit_match.c")
list(REMOVE_ITEM PCRE2_MODULES_SOURCE "${PCRE2_SOURCE_DIR}/src/pcre2_jit_misc.c")
list(REMOVE_ITEM PCRE2_MODULES_SOURCE "${PCRE2_SOURCE_DIR}/src/pcre2_ucptables.c")

foreach(file ${PCRE2_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(pcre2 thirdparty STATIC ${PCRE2_MODULES_SOURCE})

target_include_directories(pcre2 PUBLIC 
    ${PCRE2_SOURCE_DIR}/src
	)
#target_link_libraries(etcpak zlib) 
