#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(RVO2_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/rvo2)

file(GLOB  RVO2_MODULES_SOURCE 
    "${RVO2_SOURCE_DIR}/rvo2_2d/*.cpp" 
	"${RVO2_SOURCE_DIR}/rvo2_2d/*.h"
	"${RVO2_SOURCE_DIR}/rvo2_3d/*.cpp" 
	"${RVO2_SOURCE_DIR}/rvo2_3d/*.h"
	)

foreach(file ${RVO2_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(rvo2 thirdparty STATIC ${RVO2_MODULES_SOURCE})
target_include_directories(rvo2 PUBLIC ${RVO2_SOURCE_DIR}/rvo2_2d ${RVO2_SOURCE_DIR}/rvo2_3d)
#target_link_libraries(tinyexr zlib ) 
