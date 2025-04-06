#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
add_definitions(-DTINYEXR_USE_MINIZ=0)
set(TINYEXR_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/tinyexr)

file(GLOB  TINYEXR_MODULES_SOURCE "${TINYEXR_SOURCE_DIR}/*.cc" "${TINYEXR_SOURCE_DIR}/*.h")

foreach(file ${TINYEXR_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(tinyexr thirdparty STATIC ${TINYEXR_MODULES_SOURCE})
target_include_directories(tinyexr PUBLIC ${TINYEXR_SOURCE_DIR})
target_link_libraries(tinyexr zlib ) 
