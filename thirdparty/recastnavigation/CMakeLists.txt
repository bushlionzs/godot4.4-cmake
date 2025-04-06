#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(RECASTNAVIGATION_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/recastnavigation)

file(GLOB  RECASTNAVIGATION_MODULES_SOURCE 
    "${RECASTNAVIGATION_SOURCE_DIR}/Recast/source/*.cpp" 
	"${RECASTNAVIGATION_SOURCE_DIR}/Recast/source/*.h"
	)

foreach(file ${RECASTNAVIGATION_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(recastnavigation thirdparty STATIC ${RECASTNAVIGATION_MODULES_SOURCE})
target_include_directories(recastnavigation PUBLIC ${RECASTNAVIGATION_SOURCE_DIR}/Recast/include)
#target_link_libraries(tinyexr zlib ) 
