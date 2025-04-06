#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(MISC_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/misc)

file(GLOB  MISC_MODULES_SOURCE
    "${MISC_SOURCE_DIR}/*.c"
	"${MISC_SOURCE_DIR}/*.cpp"
	"${MISC_SOURCE_DIR}/*.h"
	)

foreach(file ${MISC_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(misc thirdparty STATIC ${MISC_MODULES_SOURCE})
target_include_directories(misc PRIVATE 
    ${GODOT_ENGINE_ROOT_DIRECTORY}
	${GODOT_ENGINE_ROOT_DIRECTORY}/platform/windows
	)
target_include_directories(misc PUBLIC 
    ${MISC_SOURCE_DIR}
	)
#target_link_libraries(minizip zlib) 
