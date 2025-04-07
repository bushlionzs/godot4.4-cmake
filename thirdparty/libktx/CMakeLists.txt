#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)
add_definitions(-DKHRONOS_STATIC)
add_definitions(-DMINIZ_HEADER_FILE_ONLY)
set(LIBKTX_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/libktx)

file(GLOB  LIBKTX_MODULES_SOURCE 
    "${LIBKTX_SOURCE_DIR}/lib/*.c" 
	"${LIBKTX_SOURCE_DIR}/lib/*.cpp" 
	"${LIBKTX_SOURCE_DIR}/lib/*.h"
	"${LIBKTX_SOURCE_DIR}/lib/dfdutils/*.c" 
	"${LIBKTX_SOURCE_DIR}/lib/dfdutils/*.h"
	)

foreach(file ${LIBKTX_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()
godot_add_library(libktx thirdparty STATIC ${LIBKTX_MODULES_SOURCE})
target_include_directories(libktx PRIVATE ${LIBKTX_SOURCE_DIR}/utils)
target_include_directories(libktx PUBLIC ${LIBKTX_SOURCE_DIR}/include ${LIBKTX_SOURCE_DIR}/other_include ${LIBKTX_SOURCE_DIR}/lib)
target_link_libraries(libktx zstd vulkan basis_universal)
