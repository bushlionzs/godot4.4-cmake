#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
add_definitions(-D__SSE2__)
add_definitions(-D__SSE__)
add_definitions(-DTASKING_INTERNAL)
add_definitions(-DEMBREE_LOWEST_ISA)
add_definitions(-DEMBREE_TARGET_SSE2)


set(EMBREE_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/embree)

file(GLOB  EMBREE_MODULES_SOURCE 
    "${EMBREE_SOURCE_DIR}/common/algorithms/*.cpp"
	"${EMBREE_SOURCE_DIR}/common/algorithms/*.h"
	"${EMBREE_SOURCE_DIR}/common/lexers/*.cpp"
	"${EMBREE_SOURCE_DIR}/common/lexers/*.h"
	"${EMBREE_SOURCE_DIR}/common/math/*.cpp"
	"${EMBREE_SOURCE_DIR}/common/math/*.h"
	"${EMBREE_SOURCE_DIR}/common/simd/*.cpp"
	"${EMBREE_SOURCE_DIR}/common/simd/*.h"
	"${EMBREE_SOURCE_DIR}/common/sys/*.cpp"
	"${EMBREE_SOURCE_DIR}/common/sys/*.h"
	"${EMBREE_SOURCE_DIR}/common/tasking/*.cpp"
	"${EMBREE_SOURCE_DIR}/common/tasking/*.h"
	"${EMBREE_SOURCE_DIR}/kernels/builders/*.cpp"
	"${EMBREE_SOURCE_DIR}/kernels/builders/*.h"
	"${EMBREE_SOURCE_DIR}/kernels/bvh/*.cpp"
	"${EMBREE_SOURCE_DIR}/kernels/bvh/*.h"
	"${EMBREE_SOURCE_DIR}/kernels/common/*.cpp"
	"${EMBREE_SOURCE_DIR}/kernels/common/*.h"
	"${EMBREE_SOURCE_DIR}/kernels/geometry/*.cpp"
	"${EMBREE_SOURCE_DIR}/kernels/geometry/*.h"
	"${EMBREE_SOURCE_DIR}/kernels/subdiv/*.cpp"
	"${EMBREE_SOURCE_DIR}/kernels/subdiv/*.h"
	)

foreach(file ${EMBREE_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(embree thirdparty STATIC ${EMBREE_MODULES_SOURCE})

target_include_directories(embree PUBLIC 
    ${EMBREE_SOURCE_DIR}/include
	)
#target_link_libraries(etcpak zlib) 
