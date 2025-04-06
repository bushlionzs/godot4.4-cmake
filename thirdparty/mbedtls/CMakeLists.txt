#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(MBEDTLS_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/mbedtls)

file(GLOB  MBEDTLS_MODULES_SOURCE 
    "${MBEDTLS_SOURCE_DIR}/library/*.c"
	"${MBEDTLS_SOURCE_DIR}/library/*.h"
	)


foreach(file ${MBEDTLS_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(mbedtls thirdparty STATIC ${MBEDTLS_MODULES_SOURCE})

target_include_directories(mbedtls PUBLIC 
    ${MBEDTLS_SOURCE_DIR}/include
	)
#target_link_libraries(mbedtls libogg) 
