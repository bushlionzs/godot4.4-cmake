#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(ENET_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/enet)

file(GLOB  ENET_MODULES_SOURCE 
    "${ENET_SOURCE_DIR}/*.c"
	"${ENET_SOURCE_DIR}/*.cpp"
	"${ENET_SOURCE_DIR}/*.h"
	)


foreach(file ${ENET_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(enet thirdparty STATIC ${ENET_MODULES_SOURCE})

target_include_directories(enet PRIVATE 
	${GODOT_ENGINE_ROOT_DIRECTORY}
	${GODOT_ENGINE_ROOT_DIRECTORY}/platform/windows
	)
target_include_directories(enet PUBLIC 
    ${ENET_SOURCE_DIR}
	)
#target_link_libraries(mbedtls libogg) 
