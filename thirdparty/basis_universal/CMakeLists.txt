#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
#add_definitions(-DMINIZ_NO_ZLIB_APIS)
set(BASIS_UNIVERSAL_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/basis_universal)

file(GLOB  BASIS_UNIVERSAL_MODULES_SOURCE 
    "${BASIS_UNIVERSAL_SOURCE_DIR}/encoder/*.cpp" 
    "${BASIS_UNIVERSAL_SOURCE_DIR}/encoder/*.h"
    "${BASIS_UNIVERSAL_SOURCE_DIR}/encoder/3rdparty/*.cpp" 
    "${BASIS_UNIVERSAL_SOURCE_DIR}/encoder/3rdparty/*.h"
    "${BASIS_UNIVERSAL_SOURCE_DIR}/transcoder/*.cpp" 
    "${BASIS_UNIVERSAL_SOURCE_DIR}/transcoder/*.h" )

foreach(file ${BASIS_UNIVERSAL_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(basis_universal thirdparty STATIC ${BASIS_UNIVERSAL_MODULES_SOURCE})
target_include_directories(basis_universal PUBLIC ${BASIS_UNIVERSAL_SOURCE_DIR})
target_link_libraries(basis_universal zstd jpeg-compressor tinyexr) 

