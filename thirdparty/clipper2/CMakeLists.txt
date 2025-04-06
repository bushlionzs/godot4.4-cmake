#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

set(CLIPPER2_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/clipper2)

file(GLOB  CLIPPER2_MODULES_SOURCE 
    "${CLIPPER2_SOURCE_DIR}/src/*.cpp"
	"${CLIPPER2_SOURCE_DIR}/src/*.h"
	
	)

foreach(file ${CLIPPER2_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(clipper2 thirdparty STATIC ${CLIPPER2_MODULES_SOURCE})

target_include_directories(clipper2 PUBLIC 
    ${CLIPPER2_SOURCE_DIR}/include
	)
#target_link_libraries(etcpak zlib) 
