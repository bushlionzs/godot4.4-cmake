#Enforce the highest possible version that the current Ubuntu LTS provides
#by default. We want to be sure to have the ability to attach as many things
#to the target as modern cmake code affords us
cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

add_definitions(-DXR_OS_WINDOWS)

set(OPENXR_SOURCE_DIR ${GODOT_SOURCE_ROOT_THIRDPARTY_DIR}/openxr)

file(GLOB  OPENXR_MODULES_SOURCE 
    "${OPENXR_SOURCE_DIR}/src/*.c" 
	"${OPENXR_SOURCE_DIR}/src/*.h"
    "${OPENXR_SOURCE_DIR}/src/common/*.cpp" 
	"${OPENXR_SOURCE_DIR}/src/common/*.h"
	"${OPENXR_SOURCE_DIR}/src/loader/*.cpp" 
	"${OPENXR_SOURCE_DIR}/src/loader/*.h"
	"${OPENXR_SOURCE_DIR}/src/external/jsoncpp/src/lib_json/*.cpp" 
	"${OPENXR_SOURCE_DIR}/src/external/jsoncpp/src/lib_json/*.h"
	)

foreach(file ${OPENXR_MODULES_SOURCE})
    file(RELATIVE_PATH relative_path "${CMAKE_CURRENT_SOURCE_DIR}" "${file}")
    get_filename_component(dir_path "${relative_path}" DIRECTORY)
    string(REPLACE "/" "\\" vs_filter "${dir_path}")
    source_group("${vs_filter}" FILES "${file}")
endforeach()

godot_add_library(openxr thirdparty STATIC ${OPENXR_MODULES_SOURCE})
target_include_directories(openxr PRIVATE 
    "${OPENXR_SOURCE_DIR}/src/common" 
	"${OPENXR_SOURCE_DIR}/src/loader"
	)

target_include_directories(openxr PRIVATE 
    ${OPENXR_SOURCE_DIR}/src/external/jsoncpp/include
	${OPENXR_SOURCE_DIR}/src)	
target_include_directories(openxr PUBLIC ${OPENXR_SOURCE_DIR}/include)
#target_link_libraries(tinyexr zlib ) 
