#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
add_definitions(-DMANIFOLD_PAR=-1)
set(MANIFOLD_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/manifold)

file(GLOB  MANIFOLD_MODULES_SOURCE
    "${MANIFOLD_SOURCE_DIR}/src/*.cpp"
	"${MANIFOLD_SOURCE_DIR}/src/*.h"
	)

list(REMOVE_ITEM LIBVORBIS_MODULES_SOURCE "${MANIFOLD_SOURCE_DIR}/src/sdf.cpp")

foreach(file ${MANIFOLD_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(manifold thirdparty STATIC ${MANIFOLD_MODULES_SOURCE})
target_include_directories(manifold PRIVATE 
    ${MANIFOLD_SOURCE_DIR}
	)
target_include_directories(manifold PUBLIC 
    ${MANIFOLD_SOURCE_DIR}/include
	)
#target_link_libraries(minizip zlib) 
